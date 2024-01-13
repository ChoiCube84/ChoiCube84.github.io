---
layout: post
title: "Rusty Surfers 프로젝트 개발일지 2일차"
subtitle: "점프 능력 추가"
date: 2024-01-13 23:59:00+0900
background: '/img/posts/rusty_surfers/rusty_surfers_project_bg.png'
katex: true
category: Project
tags: [ rusty_surfers ]
---

# Rusty Surfers 프로젝트

오늘도 Rusty Surfers 프로젝트를 진행하며 새로 짜거나 수정한 코드들에 대해 정리해볼 것이다. Rusty Surfers 게임 코드들은 내 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## 새로 추가된 코드와 기능

오늘 새롭게 배운 Rust 의 기능은 `impl` 을 활용하여 `Character` 구조체를 마치 클래스처럼 다루는 방법이었다. 이를 활용하여 플레이어의 움직임을 담당하는 로직을 `State` 의 `update` 함수에서 분리해낼 수 있었다.

그리고 플레이어의 점프를 구현하여 `jump` 함수를 추가하였다. 나중에 추가될 장애물을 피하거나 공중에 있는 아이템을 먹는 데에 활용할 수 있는 액션이다.

그 외에도 서핑보드의 모양으로 더 적합하게 원을 타원으로 바꾸거나, 임시로 사용할 색을 노란색으로 고정하는 등의 소소한 코드 수정이 있었다.

오늘 추가되거나 수정된 코드는 다음과 같다. 크게 수정된 부분들을 위주로 적어두었다.


### main.rs

```rust
impl ggez::event::EventHandler<GameError> for State {
    fn update(&mut self, ctx: &mut Context) -> GameResult {
        const SPEED: f32 = 5.0;

        if ctx.keyboard.is_key_pressed(KeyCode::Left) {
            self.character.move_left(SPEED);
        }
        if ctx.keyboard.is_key_pressed(KeyCode::Right) {
            self.character.move_right(SPEED);
        }
        if ctx.keyboard.is_key_just_pressed(KeyCode::Up) {
            self.character.move_upper_lane();
        }
        
        if ctx.keyboard.is_key_just_pressed(KeyCode::Down) {
            self.character.move_lower_lane();
        }
        if ctx.keyboard.is_key_just_pressed(KeyCode::Space) {
            self.character.jump();
        }

        self.character.fall_by_gravity();

        Ok(())
    }
    fn draw(&mut self, ctx: &mut Context) -> GameResult {
        let mut canvas = graphics::Canvas::from_frame(ctx, graphics::Color::BLACK);

        let semi_major_axis = 50.0 * (1.0 + 0.2 * self.character.pos_z as f32);

        let lane_divider1 = graphics::Mesh::new_line(
            ctx, 
            &[
                mint::Point2{x: 0.0, y: ((self.window_height / 2) - 100) as f32}, 
                mint::Point2{x: self.window_width as f32, y: ((self.window_height / 2) - 100) as f32}
            ],
            1.0, 
            graphics::Color::WHITE,
        )?;

        let lane_divider2 = graphics::Mesh::new_line(
            ctx, 
            &[
                mint::Point2{x: 0.0, y: ((self.window_height / 2) + 100) as f32}, 
                mint::Point2{x: self.window_width as f32, y: ((self.window_height / 2) + 100) as f32}
            ],
            1.0, 
            graphics::Color::WHITE,
        )?;

        let surfing_board = graphics::Mesh::new_ellipse(
            ctx,
            graphics::DrawMode::fill(),
            self.character.get_2d_coordinate(self.window_height),
            semi_major_axis,
            semi_major_axis * (3.0 / 16.0),
            0.1,
            graphics::Color::YELLOW,
        )?;

        canvas.draw(&lane_divider1, graphics::DrawParam::default());
        canvas.draw(&lane_divider2, graphics::DrawParam::default());

        canvas.draw(&surfing_board, graphics::DrawParam::default());

        canvas.finish(ctx)?;
        Ok(())
    }
}
```

### character.rs

```rust
use std::cmp;
use ggez::mint;

#[derive(Copy, Clone)]
pub struct Character {
    pub pos_x : f32,
    pub pos_y : f32,
    pub pos_z: i32,

    velocity_y : f32,
    acceleration_y : f32,
    on_the_air : bool,
}

impl Character {
    pub fn new(window_width : i32) -> Character {
        Character {
            pos_x : (window_width / 2) as f32,
            pos_y : 0.0,
            pos_z: 0,

            velocity_y : 0.0,
            acceleration_y : 0.0,
            on_the_air : false,
        }
    }

    pub fn fall_by_gravity(&mut self) {
        if self.on_the_air {
            self.acceleration_y = -0.3;
            self.velocity_y += self.acceleration_y;
            self.pos_y += self.velocity_y;

            if self.pos_y <= 0.0 {
                self.acceleration_y = 0.0;
                self.velocity_y = 0.0;
                self.pos_y = 0.0;

                self.on_the_air = false;
            }
        }
    }

    pub fn move_left(&mut self, speed : f32) {
        self.pos_x -= speed * (0.8 + 0.2 * self.pos_z as f32);
        self.pos_x = f32::max(self.pos_x, 50.0);
    }

    pub fn move_right(&mut self, speed : f32) {
        self.pos_x += speed * (0.8 + 0.2 * self.pos_z as f32);
        self.pos_x = f32::min(self.pos_x, 800.0 - 50.0);
    }

    pub fn move_upper_lane(&mut self) {
        if !self.on_the_air {
            self.pos_z = cmp::max(self.pos_z - 1, -1);    
        }
    }

    pub fn move_lower_lane(&mut self) {
        if !self.on_the_air {
            self.pos_z = cmp::min(self.pos_z + 1, 1);
        }
    }

    pub fn jump(&mut self) {
        if !self.on_the_air {
            self.velocity_y = 10.0;
            self.on_the_air = true;
        }
    }

    pub fn get_2d_coordinate(self, window_height : i32) -> mint::Point2<f32> {
        let base_y = (window_height / 2) + self.pos_z * 200;
        
        mint::Point2{
            x: self.pos_x, 
            y: (base_y as f32) - (1.0 + 0.2 * self.pos_z as f32) * self.pos_y
        }
    }
}
```

## 마무리

생각보다 점프가 하루 만에 쉽게 구현이 되어서 조금 놀랐다. 고등학교 때 비슷한 걸 코딩해 본 경험이 있어서 그런 것 같다.

개발을 진행하면서 TRPL<sup>[2](#footnote_2)</sup>도 간간히 읽어보고 있는데, 기존에 내가 사용 언어들과 적당히 비슷한 점들이 있어 코딩이 가능한 것 같다. 걱정되는 점은, Rust 만의 강력한 기능들을 내가 제대로 사용하고 있는지 모르겠다. 

애초에 Rust 의 '강력한 기능' 이라는게 뭔지도 잘 모르겠지만, 그런 기능들을 제대로 익히고 활용하지 못한다면 Rust 를 제대로 공부한다고 할 수 없을 것이다. 개발과 공부의 적당한 균형점을 찾을 수 있도록 고민해봐야겠다.

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/rusty-surfers>  
<a name="footnote_2">2</a>: <https://doc.rust-lang.org/book/>  