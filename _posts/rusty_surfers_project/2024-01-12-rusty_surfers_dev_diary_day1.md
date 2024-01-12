---
layout: post
title: "Rusty Surfers 프로젝트 개발일지 1일차"
subtitle: "게임 플레이 방식"
date: 2024-01-12 23:56:00+0900
background: '/img/posts/rusty_surfers/rusty_surfers_project_bg.png'
katex: true
category: Project
tags: [ rusty_surfers ]
---

# Rusty Surfers 프로젝트

오늘부터 Rusty Surfers 프로젝트를 진행하며 새로 짜거나 수정한 코드들에 대해 정리해볼 것이다. Rusty Surfers 게임 코드들은 내 깃헙 레포지토리<sup>[1](#footnote_1)</sup>를 참고하면 된다.

## Rusty Surfers 게임 플레이 방식

서핑 게임을 만들겠다고는 했지만, 한 번도 서핑을 해본 적인 없어서 유튜브로 서핑 영상을 조금 봤다. 내가 본 영상에서는 서핑 보드에 엎드린 채 물 안쪽으로 들어간 뒤, 보드 위에 선 다음, 파도에 밀려가며 서핑을 하고 있었다. 이를 이용하여 게임 진행 방식을 간단하게 정리해보았다.

1. 파도가 왼쪽에서 밀려오는 것으로 하고, 오른쪽으로 계속 이동하는 방식으로 게임이 진행된다.

2. 수영장의 레인처럼 총 3개의 레인 사이를 오고가며 서핑이 진행된다.

3. 기본 이동, 점프, 대쉬 등의 액션을 이용하여 장애물을 피하며 아이템을 먹는 방식으로 게임이 진행된다.

여기까지가 오늘 떠올린 게임의 개략적인 아이디어이다. 게임을 실제로 개발하면서 달라질 수 있다.

## 새로 추가된 코드와 기능

어제 간단히 만들어본 테스트 코드를 기반으로 레인 3개를 사이를 오고가며, 왼쪽과 오른쪽으로 이동할 수 있도록 코드를 만들어보았다.

그리고 조종하는 캐릭터의 물리적인 정보를 저장하기 위해 `Character` 이라는 구조체를 추가하였다. 이름이나 구성이 개발이 진행됨에 따라 달라질 수 있다.

오늘 추가되거나 수정된 코드는 다음과 같다.


### main.rs

```rust
use ggez::*;
use ggez::input::keyboard::KeyCode;
use ggez::conf::*;
use std::cmp;

mod character;
use character::Character;

struct State {
    window_width : i32,
    window_height : i32,

    character : Character,

    color : usize,
}

impl ggez::event::EventHandler<GameError> for State {
    fn update(&mut self, ctx: &mut Context) -> GameResult {
        const SPEED: f32 = 5.0;

        if ctx.keyboard.is_key_pressed(KeyCode::Right) {
            self.character.pos_x += SPEED * (0.8 + 0.2 * self.character.pos_z as f32);
        }
        if ctx.keyboard.is_key_pressed(KeyCode::Left) {
            self.character.pos_x -= SPEED * (0.8 + 0.2 * self.character.pos_z as f32);
        }
        if ctx.keyboard.is_key_just_pressed(KeyCode::Up) {
            self.character.pos_z = cmp::max(self.character.pos_z - 1, -1);
        }
        
        if ctx.keyboard.is_key_just_pressed(KeyCode::Down) {
            self.character.pos_z = cmp::min(self.character.pos_z + 1, 1);
        }

        if ctx.keyboard.is_key_just_pressed(KeyCode::Space) {
            self.color += 1;
            self.color %= 4;
        }

        Ok(())
    }
    fn draw(&mut self, ctx: &mut Context) -> GameResult {
        let color_vec = Vec::from([graphics::Color::RED, graphics::Color::YELLOW, graphics::Color::GREEN, graphics::Color::BLUE]);
        let mut canvas = graphics::Canvas::from_frame(ctx, graphics::Color::BLACK);

        let circle = graphics::Mesh::new_circle(
            ctx,
            graphics::DrawMode::fill(),
            mint::Point2{x: self.character.pos_x, y: ((self.window_height / 2) + self.character.pos_z * 200) as f32},
            50.0 * (0.8 + 0.2 * self.character.pos_z as f32),
            0.1,
            color_vec[self.color],
        )?;

        canvas.draw(&circle, graphics::DrawParam::default());
        canvas.finish(ctx)?;
        Ok(())
    }
}

fn main() {
    let window_width = 800;
    let window_height = 600;

    let cb = ggez::ContextBuilder::new("rust-surfers", "ChoiCube84")
        .window_setup(conf::WindowSetup::default().title("Rusty Surfers"))
        .window_mode(conf::WindowMode::default().dimensions(window_width as f32, window_height as f32));

    let character = Character {
        pos_x : (window_width / 2) as f32,
        pos_y : 0.0,
        pos_z: 0,
        on_the_air : false,
    };

    let state = State {
        window_width : window_width,
        window_height : window_height,
        
        character : character,
        
        color : 0,
    };

    let (ctx, event_loop) = cb.build().unwrap();

    event::run(ctx, event_loop, state);
}
```

### character.rs

```rust
pub struct Character {
    pub pos_x : f32,
    pub pos_y : f32,
    pub pos_z: i32,
    pub on_the_air : bool,
}
```

## 마무리

지금까지 만든 코드는 그저 원이 상하좌우로 움직이는 코드에 불과하지만, 이를 기반으로 게임의 다양한 액션을 추가해 나갈 것이다. 액션이 제대로 구현된 뒤에, 필요한 기능들을 넣기 시작할 계획이다.

생전 써본적 없는 Rust 로 프로그래밍을 하려니 머리가 무척 아팠다. Rust 문법도 모르는데 외부 라이브러리인 ggez 까지 사용하려니 조금 부담이 되기는 하지만, 그래도 조금씩 코드가 읽히기 시작하는 것 같다. 구글과 ChatGPT의 힘을 빌리가며 열심히 해봐야겠다. 

오늘의 개발은 여기까지!

- - -
<a name="footnote_1">1</a>: <https://github.com/ChoiCube84/rusty-surfers>  