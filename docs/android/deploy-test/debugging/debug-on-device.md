---
title: 디바이스에서 디버그
description: 이 문서에서는 물리적 Android 디바이스에서 Xamarin.Android 애플리케이션을 디버그하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 153D3746-A27F-198B-48FE-D219C0133A79
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 3ca524e451a7a4eb838805c839b33c4b9dd6bddd
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "75556537"
---
# <a name="debug-on-an-android-device"></a>Android 디바이스의 디버그

_이 아티클에서는 물리적 Android 디바이스에서 Xamarin.Android 애플리케이션을 디버그하는 방법을 설명합니다._

Mac용 Visual Studio 또는 Visual Studio를 사용하여 Android 디바이스에서 Xamarin.Android 앱을 디버그할 수 있습니다. 디바이스에서 디버그가 가능하려면 먼저 [개발을 위해 설정](~/android/get-started/installation/set-up-device-for-development.md)되고 PC나 MAC에 연결되어야 합니다.

## <a name="debug-application"></a>애플리케이션 디버그

디바이스가 컴퓨터에 연결된 후에는 Xamarin.Android 애플리케이션 디버그가 다른 Xamarin 제품 또는 .NET 애플리케이션에서와 같은 방식으로 수행됩니다. **디버그** 구성과 외부 디바이스를 IDE에서 선택했는지 확인합니다. 필요한 디버그 기호를 사용할 수 있고 IDE가 실행 중인 애플리케이션에 연결할 수 있음을 확인하는 것입니다. 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![디버그 구성 선택됨](debug-on-device-images/image1-vs.png)

다음으로 코드에서 중단점이 설정되어 있습니다.

![코드 줄에 중단점 설정](debug-on-device-images/image2-vs.png)

디바이스를 선택한 후에는 Xamarin.Android가 디바이스에 연결하고 애플리케이션을 배포한 다음 실행합니다. 중단점에 도달하면 디버거가 애플리케이션을 중지하여 애플리케이션이 다른 C# 애플리케이션과 유사한 방식으로 디버그될 수 있게 됩니다. 

![중단점 도달](debug-on-device-images/image3-vs.png)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

![디버그 구성 선택됨](debug-on-device-images/image1-xs.png)

다음으로 코드에서 중단점이 설정되어 있습니다.

![코드 줄에 중단점 설정](debug-on-device-images/image2-xs.png)

디바이스를 선택한 후에는 Xamarin.Android가 디바이스에 연결하고 애플리케이션을 배포한 다음 실행합니다. 중단점에 도달하면 디버거가 애플리케이션을 중지하여 애플리케이션이 다른 C# 애플리케이션과 유사한 방식으로 디버그될 수 있게 됩니다. 

![중단점 도달](debug-on-device-images/image3-xs.png)

-----

## <a name="summary"></a>요약

이 문서에서는 중단점을 설정하고 대상 디바이스를 선택하여 Xamarin.Android 애플리케이션을 디버그하는 방법에 대해 논의했습니다.

## <a name="related-links"></a>관련 링크

- [개발용 디바이스 설정](~/android/get-started/installation/set-up-device-for-development.md)
- [디버깅 가능한 특성 설정](~/android/deploy-test/debuggable-attribute.md)
