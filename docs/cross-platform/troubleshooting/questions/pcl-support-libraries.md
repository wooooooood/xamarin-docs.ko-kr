---
title: 어떻게 어떤 라이브러리는 PCL에 지원 됩니다 볼 수 있습니까?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: asb3993
ms.author: amburns
ms.openlocfilehash: 87f65ba2cff2d5990c32aa142f97766a76d6ba05
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919542"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>어떻게 어떤 라이브러리는 PCL에 지원 됩니다 볼 수 있습니까?

- 아래에서 다양 한 PCL 대상 플랫폼에서 지 원하는 다양 한 기능 개요를 확인할 수는 *지원 되는 기능* 이 페이지의 일부: [http://msdn.microsoft.com/library/gg597391.aspx](https://msdn.microsoft.com/library/gg597391.aspx)

- 다른 옵션은 사용 하는 [.NET 이식성 분석기](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) 기존 라이브러리를 PCL 프로필으로 변환할 수 있는지 여부를 평가할 수 있습니다.

- 세 번째 가능성을 사용할 수 있는 실제 프로 파일의 내용의 찾을 것입니다. 예를 들어 프로필 78를 사용 하 여, 이동할 수 있습니다 여기: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` 내의 모든 어셈블리를 확인 합니다.

어떤 방법의 선택 하십시오 NuGet 및 Microsoft BCL 라이브러리를 통해 다운로드 하는 일부 기능이 참고 합니다.
