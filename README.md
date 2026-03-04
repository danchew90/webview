# RN WebView Bridge Demo (for Vercel)

이 프로젝트는 Expo React Native 앱의 `react-native-webview`와 통신하기 위한 웹 페이지입니다.

## 배포

이 저장소를 Vercel에 연결하면 루트의 `index.html`이 바로 배포됩니다.

## React Native 연결 방법

Expo 코드의 URL을 배포 주소로 바꿔주세요.

```tsx
const VERCEL_URL = 'https://your-app.vercel.app';
```

WebView 설정은 질문에서 주신 코드 그대로 사용하되,
웹에서는 아래 방식으로 RN으로 전송합니다.

- Web → RN: `window.ReactNativeWebView.postMessage(JSON.stringify(payload))`
- RN → Web: RN의 `webViewRef.current?.postMessage(payload)` 를 웹에서 `message` 이벤트로 수신

## 로컬 확인

```bash
python3 -m http.server 4173
```

그다음 브라우저에서 `http://localhost:4173` 접속해서 동작을 확인할 수 있습니다.
