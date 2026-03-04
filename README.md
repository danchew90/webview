# RN WebView Bridge Demo (for Vercel)

이 프로젝트는 Expo React Native 앱의 `react-native-webview`와 통신하기 위한 웹 페이지입니다.

## 배포

이 저장소를 Vercel에 연결하면 루트의 `index.html`이 바로 배포됩니다.

## 이번 개선 포인트

- 웹 화면을 모바일에서도 보기 편하게 반응형(`row` → 세로 스택)으로 개선
- 전송 로직 공통화(`postToRN`)로 코드 단순화
- `type: "URL"` 전용 버튼/입력창 추가
  - 버튼 클릭 시 RN으로 아래 payload 전송

```json
{
  "type": "URL",
  "url": "https://example.com",
  "timestamp": "오후 9:10:00"
}
```

## Expo Snack 쪽 최적화 예시

아래처럼 RN에서 `onMessage`를 확장하면, 웹에서 보낸 URL 타입을 바로 처리할 수 있습니다.

```tsx
const handleMessage = (event) => {
  const raw = event.nativeEvent.data;

  try {
    const parsed = JSON.parse(raw);

    if (parsed.type === 'URL' && parsed.url) {
      addMessage('WEB → RN (URL)', parsed.url, '#FF9800');
      // 필요하면 여기서 Linking.openURL(parsed.url) 같은 처리 가능
      return;
    }

    addMessage('WEB → RN', parsed.message || raw, '#2196F3');
  } catch {
    addMessage('WEB → RN', raw, '#2196F3');
  }
};
```

그리고 WebView 주소는 실제 배포 주소로 맞춰주세요.

```tsx
const VERCEL_URL = 'https://webview-omega.vercel.app/';
```

## 로컬 확인

```bash
python3 -m http.server 4173
```

브라우저에서 `http://localhost:4173` 접속 후 아래를 확인하세요.

1. 일반 메시지 전송 버튼
2. URL 전송 버튼 (`type: "URL"`)
3. RN → WEB 시뮬레이션 버튼
