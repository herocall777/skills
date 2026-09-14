---
name: lyric-music-video
description: AI로 생성한 음악과 이미지를 활용해, 가사가 노래 타이밍에 맞춰 화면에 나타나는 유튜브 플레이리스트용 가사 영상(Lyric Video)을 Remotion으로 제작하는 방법
version: 1.0.0
author: herocall1 (제이린쌤 AI 뮤직 스쿨)
based-on: remotion-dev/skills (remotion-best-practices)
---

이 스킬은 `remotion-best-practices`의 하위 스킬들을 조합해서, "음악이 흘러나오면서 가사가 싱크에 맞춰 나오는 영상"을 만드는 전용 워크플로우를 제공합니다. 유튜브 플레이리스트 채널용 AI 음악 영상 제작에 최적화되어 있습니다.

## 언제 이 스킬을 쓰나요

- Suno 등으로 만든 AI 음악 mp3 + 가사 텍스트가 있고, 이를 가사 싱크 영상(리릭 비디오)으로 만들고 싶을 때
- 배경 이미지/영상 위에 가사 자막이 줄 단위 또는 단어 단위로 하이라이트되며 나오는 영상을 원할 때
- 유튜브 업로드용(16:9) 또는 쇼츠용(9:16) 포맷 모두 대응

## 준비물

1. 오디오 파일 (mp3/wav) — AI로 생성한 곡
2. 가사 타이밍 데이터 — 아래 두 방법 중 하나
   - (A) 자동 생성: 오디오를 자동 전사해서 타이밍 추출
   - (B) 수동 작성: 가사와 시작/끝 시간을 직접 SRT나 JSON으로 작성 (AI로 만든 곡은 자동 전사 정확도가 낮을 수 있어 수동 방식을 추천)
3. 배경 소스 — 이미지 여러 장(앨범아트 스타일) 또는 짧은 루프 영상

## 단계

### 1. 프로젝트 생성
`remotion-create` 스킬을 로드해서 새 Remotion 프로젝트를 만들거나 기존 프로젝트에 새 Composition을 추가합니다.

### 2. 오디오 삽입
`remotion-markup/audio.md`를 참고해 `<Audio>` 컴포넌트로 mp3를 불러옵니다. `remotion-multimedia/get-audio-duration.md`로 오디오 길이를 읽어 Composition의 `durationInFrames`를 자동 계산하세요.

### 3. 가사 타이밍 데이터 준비
- 자동 전사가 필요하면 `remotion-captions/transcribe-captions.md`를 로드하세요.
- 이미 가사와 타이밍(줄 단위 시작 시간)을 알고 있다면, 직접 `Caption[]` 형태의 JSON으로 작성하는 게 가장 정확합니다.
- SRT 파일이 있다면 `remotion-captions/import-srt-captions.md`를 로드해 가져옵니다.

### 4. 가사 화면 표시 (싱크 애니메이션)
- 기본 표시 로직은 `remotion-captions/display-captions.md`를 로드하세요.
- 현재 부르고 있는 단어를 하이라이트(가라오케 스타일)하고 싶다면 `remotion-markup/text-highlights.md`를 로드하세요.
- 줄바꿈/등장·퇴장 타이밍을 세밀히 제어하려면 `remotion-markup/timing.md`, `remotion-markup/sequencing.md`를 참고하세요.

### 5. 배경 비주얼 구성
- 정지 이미지를 서서히 확대/이동시키는 효과(Ken Burns)는 `remotion-markup/images.md`를 참고하세요.
- 음악에 반응하는 파형/스펙트럼 비주얼라이저를 넣으려면 `remotion-markup/audio-visualization.md`를 로드하세요.
- 여러 장면(인트로 → 벌스 → 코러스)을 전환하려면 `remotion-markup/multi-scene-video.md`와 `remotion-markup/transitions.md`를 참고하세요.

### 6. 미리보기
`remotion-studio` 스킬로 Remotion Studio를 실행해 가사 싱크가 정확한지 확인합니다.

### 7. 렌더링
`remotion-render` 스킬로 최종 mp4를 출력합니다. 유튜브 일반 영상은 1920x1080, 쇼츠는 1080x1920으로 Composition을 설정하세요.

## 가사 데이터 형식

`@remotion/captions`의 `Caption` 타입을 그대로 재사용합니다:

```ts
type Caption = {
  text: string;       // 가사 한 줄 또는 한 단어
  startMs: number;    // 시작 시간(ms)
  endMs: number;      // 끝 시간(ms)
  timestampMs: number | null;
  confidence: number | null;
  pageBreakAfter?: boolean;
};
```

예시 스켈레톤 코드는 `assets/lyric-video-example.md`를 참고하세요.

## 체크리스트

- [ ] 오디오 길이와 Composition `durationInFrames`가 일치하는가
- [ ] 가사 타이밍이 실제 보컬과 어긋나지 않는가 (Studio에서 재생하며 확인)
- [ ] 마지막 줄 이후 아웃트로 여백이 있는가
- [ ] 16:9 / 9:16 등 목적에 맞는 해상도로 렌더링했는가
