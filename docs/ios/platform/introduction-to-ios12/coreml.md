---
title: Xamarin.iOS에서 ML 2 코어
description: 이 문서에서는 iOS 12의 일부로 사용할 수 있는 기계 학습 Core에 대 한 업데이트를 설명합니다. 특히 새 일괄 처리 예측 API와 관련 된 성능 향상에 살펴봅니다.
ms.prod: xamarin
ms.assetid: 408E752C-2C78-4B20-8B43-A6B89B7E6D1B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/15/2018
ms.openlocfilehash: 50d59f0b6ff2133c5870d84a1d740547768116e0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61398847"
---
# <a name="core-ml-2-in-xamarinios"></a>Xamarin.iOS에서 ML 2 코어

코어 ML는 machine learning 기술 iOS, macOS, tvOS 및 watchOS에서 사용할 수 있습니다. 기계 학습 모델을 기반으로 예측을 수행 하는 앱 수 있습니다.

IOS 12, 핵심 ML 일괄 처리 API를 포함 합니다. 이 API는 핵심 ML이 더 효율적 하 고 모델은 예측의 시퀀스 수 있도록 사용 시나리오에서 성능 향상을 제공 합니다.

## <a name="sample-app-marshabitatcoremltimer"></a>샘플 앱: MarsHabitatCoreMLTimer

코어 ML을 사용 하 여 일괄 처리 예측을 보여 주기 위해 잠시 살펴 합니다 [MarsHabitatCoreMLTimer](https://developer.xamarin.com/samples/monotouch/iOS12/MarsHabitatCoreMLTimer) 샘플 앱입니다. 이 샘플에서는 핵심 ML 모델을 habitat Mars에서 구축 비용을 예측 하도록 학습 입력을 기준으로 다양 한: 태양, greenhouses, 개수 및 acres 수 수 있습니다.

이 문서의 코드 조각은이 샘플에서 제공 됩니다.

## <a name="generate-sample-data"></a>예제 데이터 생성

`ViewController`, 샘플 앱의 `ViewDidLoad` 메서드 호출 `LoadMLModel`, 포함 된 핵심 ML 모델을 로드 하는:

```csharp
void LoadMLModel()
{
    var assetPath = NSBundle.MainBundle.GetUrlForResource("CoreMLModel/MarsHabitatPricer", "mlmodelc");
    model = MLModel.Create(assetPath, out NSError mlErr);
}
```

그런 다음 샘플 앱을 만듭니다 100,000 `MarsHabitatPricerInput` 순차 핵심 ML 예측에 대 한 입력으로 사용할 개체입니다. 각 생성 된 샘플 태양, greenhouses, 개수 및 acres 수가 수에 대해 설정 된 임의 값에 있습니다.

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

예측의 두 시퀀스를 실행 하는 앱의 세 가지 단추 중 하나를 탭 합니다: 하나를 사용 하 여는 `for` 루프 및 새 일괄 처리를 사용 하 여 다른 `GetPredictions` iOS 12에에서 도입 된 메서드:

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

합니다 `for` 잡이 지정된 된 수의 입력 루프 버전의 테스트를 반복 호출 [ `GetPrediction` ](xref:CoreML.MLModel.GetPrediction*) 각 하 고 결과 삭제 합니다. 메서드는 예측을 수행 하는 데 걸리는 시간:

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

## <a name="getpredictions-new-batch-api"></a>GetPredictions (새 일괄 처리 API)

테스트의 일괄 처리 버전 만듭니다는 `MLArrayBatchProvider` 입력된 배열에서 개체 (에 필요한 입력된 매개 변수 이므로 `GetPredictions` 메서드)를 만듭니다는 [`MLPredictionOptions`](xref:CoreML.MLPredictionOptions)
CPU로 제한 하는 예측 계산을 방지 하 고 사용 하는 개체는 `GetPredictions` 다시 결과 삭제 하는 예측을 인출 하는 API:

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

시뮬레이터와 장치 모두에서 `GetPredictions` 루프 기반 핵심 ML 예측 보다 더 빨리 완료 합니다.

## <a name="related-links"></a>관련 링크

- [샘플 앱 – MarsHabitatCoreMLTimer](https://developer.xamarin.com/samples/monotouch/iOS12/MarsHabitatCoreMLTimer)
- [코어 ML, 1 부 (WWDC 2018)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/708/)
- [코어 ML, 2 부 (WWDC 2018)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/709/)
- [Xamarin.iOS에서 ML Core 소개](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/coreml)
- [코어 ML (Apple)](https://developer.apple.com/documentation/coreml?language=objc)
- [코어 ML 모델을 사용 하 여 작업](https://developer.apple.com/machine-learning/build-run-models/)
