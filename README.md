# bergwerk-media-2

베르크서울 상품 영상과 첫 프레임 포스터를 jsDelivr CDN으로 제공하는 두 번째 공개 미디어 저장소입니다.

## 업로드 저장소 전환

- **2026-10-16 00:00 KST부터 신규 업로드는 `seoraksanredcrayfish/bergwerk-media-2`를 사용합니다.** 촬영일이 아니라 업로드 시점 기준입니다.
- 기존 [`bergwerk-media`](https://github.com/seoraksanredcrayfish/bergwerk-media)의 영상·포스터·CDN URL은 그대로 유지합니다. 이전 파일을 복사·이동하거나 기존 상품의 링크를 일괄 변경하지 않습니다.
- 기존 영상의 재인코딩·교체·재시도는 원래 저장소와 경로를 사용해 기존 URL을 보존합니다.
- 저장소만 분리하며 영상 변환, 파일명, 캡슐 폴더, 포스터, CDN 검증, 라이브러리 등록 절차는 기존과 같습니다.

## 저장소 변수

`media-upload` 스킬과 업로드 실행체는 몰별 `ops.media_repo` 설정을 읽습니다. 저장소 이름을 코드에 하드코딩하지 않습니다.

| 설정 | 신규 베르크 업로드 값 |
|---|---|
| `tenant_id` | `bergwerkseoul` |
| `repo` / `MEDIA_REPO` | `seoraksanredcrayfish/bergwerk-media-2` |
| `branch` / `MEDIA_BRANCH` | `main` |
| `cdn_base` / `MEDIA_CDN_BASE` | `https://cdn.jsdelivr.net/gh/seoraksanredcrayfish/bergwerk-media-2@main/` |
| `max_bytes` | 기존 몰별 설정 유지 (베르크: 20,971,520 bytes) |

위 대문자 이름은 문서에서 설정을 가리키는 변수명이며, 실행체에 새 환경변수 지원이 생겼다는 뜻이 아닙니다. 실제 실행값은 `ops.media_repo` 행에서 읽습니다. 신규 업로드 전에 `repo`, `branch`, `cdn_base`가 위 값과 일치하는지 확인하고, 불일치하면 구 저장소로 업로드하지 말고 설정부터 맞춥니다. 기존 `ops.media_asset` 행의 URL은 변경하지 않습니다.

## 경로와 CDN

```text
<캡슐>/<룩-접두사(선택)><원본파일명>.mp4
<캡슐>/<룩-접두사(선택)><원본파일명>.jpg

https://cdn.jsdelivr.net/gh/<MEDIA_REPO>@<MEDIA_BRANCH>/<repo_path>
https://purge.jsdelivr.net/gh/<MEDIA_REPO>@<MEDIA_BRANCH>/<repo_path>
```

- 캡슐이 지정되지 않으면 `uncategorized`를 사용합니다. 임의로 캡슐·룩을 추정하지 않습니다.
- 원본은 보존합니다. 영상은 MP4/H.264, 무음, `faststart`, `yuv420p`를 사용합니다.
- 기본 경로는 원본 화면 비율·표시 방향을 보존하고 업스케일하지 않습니다. 2:3 클로즈업은 요청된 경우에만 `_2x3.mp4` 추가본으로 만듭니다.
- 첫 프레임 JPG 포스터를 함께 올립니다. 배치는 커밋·푸시 한 번으로 처리하고 CDN HTTP 200 확인 후에만 라이브러리에 등록합니다.
- 배너·웹 그래픽은 [`bergwerk-web-assets`](https://github.com/seoraksanredcrayfish/bergwerk-web-assets)를 사용합니다. 홈페이지 소스, 비공개 참고자료, 인증정보는 이 저장소에 올리지 않습니다.
