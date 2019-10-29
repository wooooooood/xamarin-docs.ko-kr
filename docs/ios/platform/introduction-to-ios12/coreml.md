---
title: Xamarin.ios의 핵심 ML 2
description: 이 문서에서는 iOS 12의 일부로 사용할 수 있는 코어 ML 업데이트에 대해 설명 합니다. 특히 새로운 일괄 처리 예측 API와 관련 된 성능 향상에 대해 살펴봅니다.
ms.prod: xamarin
ms.assetid: 408E752C-2C78-4B20-8B43-A6B89B7E6D1B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/15/2018
ms.openlocfilehash: 6245873385caa23e37d5499daa822fa0b699ac1e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032031"
---
# <a name="core-ml-2-in-xamarinios"></a>Xamarin.ios의 핵심 ML 2

핵심 ML은 iOS, macOS, tvOS 및 watchOS에서 사용할 수 있는 기계 학습 기술입니다. 이를 통해 앱은 기계 학습 모델을 기반으로 예측을 만들 수 있습니다.

IOS 12에서 코어 ML에는 batch 처리 API가 포함 되어 있습니다. 이 API는 코어 ML의 효율성을 향상 시키고 모델을 사용 하 여 예측 시퀀스를 만드는 시나리오에서 성능 향상을 제공 합니다.

## <a name="sample-app-marshabitatcoremltimer"></a>샘플 앱: MarsHabitatCoreMLTimer

핵심 ML을 사용한 일괄 처리 예측을 시연 하려면 [MarsHabitatCoreMLTimer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer) 샘플 앱을 살펴보세요. 이 샘플에서는 학습 된 핵심 기계 학습 모델을 사용 하 여 다양 한 입력 (양력인 패널 수, greenhouses 수, acres 수)을 기반으로 Mars에서 habitat를 작성 하는 비용을 예측 합니다.

이 문서의 코드 조각은이 샘플에서 가져온 것입니다.

## <a name="generate-sample-data"></a>예제 데이터 생성

`ViewController`샘플 응용 프로그램의 `ViewDidLoad` 메서드는 포함 된 코어 ML 모델을 로드 하는 `LoadMLModel`를 호출 합니다.

```csharp
void LoadMLModel()
{
    var assetPath = NSBundle.MainBundle.GetUrlForResource("CoreMLModel/MarsHabitatPricer", "mlmodelc");
    model = MLModel.Create(assetPath, out NSError mlErr);
}
```

그런 다음 샘플 앱은 순차 코어 ML 예측의 입력으로 사용할 10만 `MarsHabitatPricerInput` 개체를 만듭니다. 생성 된 각 샘플에는 양력인 패널 수, greenhouses 수, acres 수에 대 한 임의 값이 설정 되어 있습니다.

```csharp
async void CreateInputs(int num)
{
    // ...
    Random r = new Random();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            double solarPanels = r.NextDouble() * MaxSolarPanels;
            double greenHouses = r.NextDouble() * MaxGreenHouses;
            double acres = r.NextDouble() * MaxAcres;
            inputs[i] = new MarsHabitatPricerInput(solarPanels, greenHouses, acres);
        }
    });
    // ...
}
```

앱의 세 단추를 누르면 두 가지 예측 시퀀스가 실행 됩니다. 하나는 `for` 루프를 사용 하 고 다른 하나는 iOS 12에 도입 된 새 batch `GetPredictions` 방법을 사용 합니다.

```csharp
async void RunTest(int num)
{
    // ...
    await FetchNonBatchResults(num);
    // ...
    await FetchBatchResults(num);
    // ...
}
```

## <a name="for-loop"></a>for 루프

테스트 naively `for` 루프 버전은 지정 된 수의 입력을 반복 하 여 각에 대 한 [`GetPrediction`](xref:CoreML.MLModel.GetPrediction*) 를 호출 하 고 결과를 삭제 합니다. 메서드는 예측을 만드는 데 소요 되는 시간을 지정 합니다.

```csharp
async Task FetchNonBatchResults(int num)
{
    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            IMLFeatureProvider output = model.GetPrediction(inputs[i], out NSError error);
        }
    });
    stopWatch.Stop();
    nonBatchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## <a name="getpredictions-new-batch-api"></a>GetPredictions (새 batch API)

테스트의 일괄 버전은 입력 배열에서 `MLArrayBatchProvider` 개체를 만듭니다 .이는 `GetPredictions` 메서드에 필요한 입력 매개 변수 이므로는를 만듭니다 [`MLPredictionOptions`](xref:CoreML.MLPredictionOptions)
예측 계산이 CPU로 제한 되지 않도록 하 고 `GetPredictions` API를 사용 하 여 예측을 인출 하 고 결과를 다시 삭제 하는 개체입니다.

```csharp
async Task FetchBatchResults(int num)
{
    var batch = new MLArrayBatchProvider(inputs.Take(num).ToArray());
    var options = new MLPredictionOptions()
    {
        UsesCpuOnly = false
    };

    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        model.GetPredictions(batch, options, out NSError error);
    });
    stopWatch.Stop();
    batchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## <a name="results"></a>결과

시뮬레이터와 장치 모두에서 `GetPredictions` 루프 기반 코어 ML 예측 보다 더 빨리 완료 됩니다.

## <a name="related-links"></a>관련 링크

- [샘플 앱-MarsHabitatCoreMLTimer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer)
- [핵심 ML의 새로운 기능, 1 부 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/708/)
- [핵심 ML의 새로운 기능, 2 부 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/709/)
- [Xamarin.ios의 핵심 ML 소개](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/coreml)
- [코어 ML (Apple)](https://developer.apple.com/documentation/coreml?language=objc)
- [핵심 ML 모델 작업](https://developer.apple.com/machine-learning/build-run-models/)
