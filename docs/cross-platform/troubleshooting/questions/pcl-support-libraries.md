---
title: PCL에서 지원되는 라이브러리를 어떻게 볼 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: conceptdev
ms.author: crdun
ms.date: 07/27/2018
ms.openlocfilehash: 31dc5114a04deaf1a35bbd24f71cfa552f61d226
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290977"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>PCL에서 지원되는 라이브러리를 어떻게 볼 수 있나요?

- 이 페이지의 *지원 되는 기능* 부분에 있는 다양 한 PCL 대상 플랫폼에서 지 원하는 다양 한 기능에 대 한 개요를 찾을 수 있습니다.[https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

- 또 다른 옵션은 [.Net 이식성 분석기](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) 를 사용 하 여 기존 라이브러리를 PCL 프로필로 변환할 수 있는지 여부를 평가 하는 것입니다.

- 세 번째 가능성은 사용할 수 있는 실제 프로필의 콘텐츠를 찾아보는 것입니다. 예를 들어 Profile 78을 사용 하 여 다음과 같은 작업을 할 수 있습니다. `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\`그리고 그 안의 모든 어셈블리를 봅니다.

선택한 방법에 상관 없이 NuGet 및 Microsoft BCL 라이브러리를 통해 일부 기능을 다운로드 해야 합니다.
