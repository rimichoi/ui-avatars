# UI Avatars (한글 지원 fork)

이름으로 이니셜 아바타 이미지를 만들어 주는 [LasseRafn/ui-avatars](https://github.com/LasseRafn/ui-avatars)의 fork입니다. 원본에서 깨지던 한글 이름을 PNG와 SVG 모두에서 제대로 그리도록 고쳤습니다.

| 원본 | 이 fork |
| --- | --- |
| ![원본: 한글이 네모로 깨짐](assets/hangul-before.png) | ![fork: 한글이 정상 표시됨](assets/hangul-after.png) |

## 바뀐 점

- **PNG**: 원본은 글자 종류에 맞춰 폰트를 고르는데, 한글이 목록에 없어 OpenSans로 그리는 바람에 네모(□)로 깨졌습니다. 이 fork는 이니셜에 한글이 있으면 라이브러리에 들어 있는 Noto CJK 폰트로 그리고, `bold=true`면 Bold 폰트를 씁니다.
- **SVG**: SVG는 보는 쪽 브라우저가 글자를 그립니다. 폰트 목록에 macOS 기본(`Apple SD Gothic Neo`), Windows 기본(`Malgun Gothic`), `Noto Sans KR`을 넣어 브라우저 대체 폰트에 맡기지 않고 한글 폰트를 직접 지정했습니다.
- 이니셜 규칙은 원본과 같습니다. 띄어쓰기가 있으면 단어마다 첫 글자를 따고, 없으면 앞에서부터 `length`만큼 잘라 씁니다. 그래서 `홍길동`은 `홍길`, `김 철수`는 `김철`이 됩니다. 다른 글자를 보이고 싶으면 `name`에 보일 글자를 직접 넣으세요(예: `name=길동`).

## 실행

```sh
docker compose up --build
```

`http://localhost:8678/api/?name=홍길동` 으로 이미지를 받을 수 있습니다. PNG는 `cache/` 폴더에 저장되고, 만든 지 2일이 지나면 매시간 도는 cron이 지웁니다.

## API

모든 요청은 이미지 그 자체를 돌려주므로 `<img src="...">`에 바로 넣으면 됩니다.

```html
<img src="http://localhost:8678/api/?name=홍길동&background=random&rounded=true">
```

| 파라미터 | 설명 | 기본값 |
| --- | --- | --- |
| `name` | 이니셜을 만들 이름. 이니셜을 직접 넣어도 됩니다. | `John Doe` |
| `size` | 이미지 크기(px). 16~512 범위를 벗어나면 가까운 끝값으로 맞춥니다. | `64` |
| `width`, `height` | 가로·세로를 따로 정할 때 씁니다. 범위는 `size`와 같습니다. | `size` 값 |
| `length` | 이니셜 글자 수 | `2` |
| `font-size` | 이미지 크기 대비 글자 크기 비율(0~1) | `0.4375` |
| `background` | 배경색 hex(`#` 없이). `random`이면 무작위 색을 고르고 글자색도 그에 맞춥니다. | `ddd` |
| `color` | 글자색 hex(`#` 없이) | `222` |
| `rounded` | `true`면 원형 | `false` |
| `bold` | `true`면 굵은 글꼴 | `false` |
| `uppercase` | `false`면 영문 대소문자를 그대로 둡니다. | `true` |
| `format` | `png` 또는 `svg` | 요청 `Accept` 헤더에 `image/svg+xml`이 있으면 `svg`, 없으면 `png` |

query string 대신 경로로 넘길 수도 있습니다. Gravatar의 `d=` 처럼 query string을 쓸 수 없는 곳에서 씁니다. 순서는 아래와 같고, 뒤쪽은 생략할 수 있습니다.

```
/api/{name}/{size}/{background}/{color}/{length}/{font-size}/{rounded}/{uppercase}/{bold}/{format}/{width}/{height}
```

예: `http://localhost:8678/api/홍길동/128/3949ab/fff`

## 원본과 라이선스

원본은 [Lasse Rafn](https://github.com/LasseRafn)과 기여자들이 만들었고, 원본 서비스는 <https://ui-avatars.com> 입니다. 이 fork도 원본과 같은 [MIT License](LICENSE)를 따릅니다.
