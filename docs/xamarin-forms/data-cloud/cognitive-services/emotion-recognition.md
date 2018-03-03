---
title: "Emotion API를 사용 하 여 emotion 인식"
description: "Emotion API 얼굴 식을 입력으로 이미지에 하며 감정 각 면 이미지에 대 한 조합에 대해 신뢰도 수준을 반환 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램을 평가 하려면 emotion 인식 하도록 Emotion API를 사용 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 159bd1b23eb7505c5d5629570a34d54e0525567e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="emotion-recognition-using-the-emotion-api"></a>Emotion API를 사용 하 여 emotion 인식

_Emotion API 얼굴 식을 입력으로 이미지에 하며 감정 각 면 이미지에 대 한 조합에 대해 신뢰도 수준을 반환 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램을 평가 하려면 emotion 인식 하도록 Emotion API를 사용 하는 방법을 설명 합니다._

![](~/media/shared/preview.png "이 API는 현재 시험판 버전")

> [!NOTE]
> Emotion API 미리 보기입니다. 있습니다 수 수의 주요 변경 내용 최종 릴리스 전에 API입니다.

## <a name="overview"></a>개요

Emotion API 분노, 및 검색할 수 컨 템, disgust, 걱정, 만족도, neutral, 슬픔, 놀랄 얼굴 식에서입니다. 이러한 감정은 보편적으로 및 문화 기본 동일한 얼굴 식을 통해 전달 됩니다. 얼굴 식에 대 한는 emotion 결과 반환 하는 데 뿐만 아니라 Emotion API도 Face API를 사용 하 여 검색 된 글꼴에 대 한 경계 상자를 반환 합니다. 사용자가 이미 Face API를 호출 하 고, 경우에 선택적 입력으로 얼굴 사각형을 제출할 수 있습니다. Emotion API를 사용 하면 API 키를 얻어야 하는 참고 합니다. 다운로드할 수 있습니다 [무료로 시작](https://www.microsoft.com/cognitive-services/sign-up) microsoft.com 합니다.

Emotion 인식 클라이언트 라이브러리를 통해 및 REST API를 통해 수행할 수 있습니다. 이 문서에서는 emotion 인식을 통해 수행 된 [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) NuGet에서 다운로드할 수 있는 클라이언트 라이브러리입니다.

Emotion API 비디오에서는 사용자의 얼굴 식을 인식할 수도 사용할 수 있으며 해당 감정의 요약 정보를 반환 합니다. 자세한 내용은 참조 [비디오에서 Emotion](https://www.microsoft.com/cognitive-services/emotion-api/documentation#emotion-in-video) microsoft.com 합니다.

Emotion API에 대 한 자세한 내용은 참조 [Emotion API 설명서](https://www.microsoft.com/cognitive-services/emotion-api/documentation) microsoft.com 합니다.

## <a name="performing-emotion-recognition"></a>Emotion 인식을 수행

Emotion 인식 Emotion API을 이미지 스트림으로 업로드 하 여 이루어집니다. 이미지 파일 크기를 4MB 보다 큰 수 해서는 안 되며 지원 되는 파일 형식은 JPEG, PNG, GIF, 및 BMP 있습니다. 내 이미지를 감지할 수 글꼴 크기 범위는 36 x 36를 4096 x 4096 픽셀입니다. 이 범위 밖의 모든 면을 검색할 수 없습니다.

다음 코드 예제에서는 emotion 인식 프로세스를 보여 줍니다.

```csharp
using Microsoft.ProjectOxford.Emotion;
using Microsoft.ProjectOxford.Emotion.Contract;

var emotionClient = new EmotionServiceClient(Constants.EmotionApiKey);

using (var photoStream = photo.GetStream())
{
  Emotion[] emotionResult = await emotionClient.RecognizeAsync(photoStream);
  if (emotionResult.Any())
  {
    // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
    emotionResultLabel.Text = emotionResult.FirstOrDefault().Scores.ToRankedList().FirstOrDefault().Key;
  }
  // Store emotion as app rating
  ...
}
```

`EmotionServiceClient` emotion 인식에 인수로 전달 되 고 Emotion API 키를 사용 하 여 수행할 하려면 인스턴스를 만들어야는 `EmotionServiceClient` 생성자입니다.

`RecognizeAsync` 에 호출 되는 메서드는 `EmotionServiceClient` 인스턴스를 이미지로 Emotion API에 업로드 한 `Stream`합니다. 이 작업을 호출 하는 경우 API 키 Emotion API에 전송 됩니다. 실패가 발생 올바른 API 키를 전송 하는 `Microsoft.ProjectOxford.Common.ClientException` 잘못 된 API 키 제출 되었음을 나타내는 예외 메시지와 함께 throw 되 고 있습니다.

`RecognizeAsync` 메서드는 반환 된 `Emotion` 는 얼굴 인식 된 배열입니다. 각 이미지에 대 한 면 감지할 수 있는 최대 수는 64, 및 얼굴 내림차순 얼굴 사각형 크기에 따라 순위가 지정 됩니다. 없는 얼굴 감지 되 면 빈 `Emotion` 배열이 반환 됩니다.

검색 된 emotion 점수를 기본 형태로 변환으로 가장 높은 점수를 emotion으로 해석 되도록 Emotion API에서 결과 해석 하는 경우 하나에 대 한 합계를 구할 합니다. 따라서 샘플 응용 프로그램 다음 스크린샷에서 같이 가장 큰 검색 된 글꼴에 대 한 가장 높은 점수를 인식 된 emotion 이미지에 표시 합니다.

![](emotion-recognition-images/emotion-recognition.png "Emotion 인식")

## <a name="summary"></a>요약

이 문서는 Xamarin.Forms 응용 프로그램을 평가 하려면 emotion 인식 하도록 Emotion API를 사용 하는 방법을 설명 합니다. Emotion API 얼굴 식을 입력으로 이미지의 하며 이미지의 각 면에 대해 감정 조합에 대해 신뢰도 반환 합니다.


## <a name="related-links"></a>관련 링크

- [Emotion API 설명서](https://www.microsoft.com/cognitive-services/emotion-api/documentation)
- [Todo Cognitive 서비스 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/)
- [Emotion API](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)
