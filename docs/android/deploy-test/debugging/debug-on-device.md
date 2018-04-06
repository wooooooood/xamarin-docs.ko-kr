---
title: 장치에서 디버그
description: 이 문서에서는 물리적 Android 장치에서 Xamarin.Android 응용 프로그램을 디버그하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 153D3746-A27F-198B-48FE-D219C0133A79
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1848bb624bf5f4bd627441a17fd077843c94edb9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="debug-on-device"></a>장치에서 디버그

_이 아티클에서는 물리적 Android 장치에서 Xamarin.Android 응용 프로그램을 디버그하는 방법을 설명합니다._

## <a name="debug-on-device-overview"></a>장치에서 디버그 개요

Mac용 Visual Studio 또는 Visual Studio를 사용하여 Android 장치에서 Xamarin.Android 앱을 디버그할 수 있습니다. 장치에서 디버그가 가능하려면 먼저 [개발을 위해 설정](~/android/get-started/installation/set-up-device-for-development.md)되고 PC나 MAC에 연결되어야 합니다.


## <a name="debug-application"></a>응용 프로그램 디버그

장치가 컴퓨터에 연결된 후에는 Xamarin.Android 응용 프로그램 디버그가 다른 Xamarin 제품 또는 .NET 응용 프로그램에서와 같은 방식으로 수행됩니다. **디버그** 구성과 외부 장치를 IDE에서 선택했는지 확인합니다. 필요한 디버그 기호를 사용할 수 있고 IDE가 실행 중인 응용 프로그램에 연결할 수 있음을 확인하는 것입니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![디버그 구성 선택됨](debug-on-device-images/image1-vs.png)

다음으로 코드에서 중단점이 설정되어 있습니다.

![코드 줄에 중단점 설정](debug-on-device-images/image2-vs.png)

장치를 선택한 후에는 Xamarin.Android가 장치에 연결하고 응용 프로그램을 배포한 다음 실행합니다. 중단점에 도달하면 디버거가 응용 프로그램을 중지하여 응용 프로그램이 다른 C# 응용 프로그램과 유사한 방식으로 디버그될 수 있게 됩니다. 

![중단점 도달](debug-on-device-images/image3-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![디버그 구성 선택됨](debug-on-device-images/image1-xs.png)

다음으로 코드에서 중단점이 설정되어 있습니다.

![코드 줄에 중단점 설정](debug-on-device-images/image2-xs.png)

장치를 선택한 후에는 Xamarin.Android가 장치에 연결하고 응용 프로그램을 배포한 다음 실행합니다. 중단점에 도달하면 디버거가 응용 프로그램을 중지하여 응용 프로그램이 다른 C# 응용 프로그램과 유사한 방식으로 디버그될 수 있게 됩니다. 

![중단점 도달](debug-on-device-images/image3-xs.png)

-----



## <a name="summary"></a>요약

이 문서에서는 중단점을 설정하고 대상 장치를 선택하여 Xamarin.Android 응용 프로그램을 디버그하는 방법에 대해 논의했습니다.


## <a name="related-links"></a>관련 링크

- [개발용 장치 설정](~/android/get-started/installation/set-up-device-for-development.md)
- [디버깅 가능한 특성 설정](~/android/deploy-test/debuggable-attribute.md)
