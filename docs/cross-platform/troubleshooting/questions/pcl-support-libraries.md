---
title: PCL에서 지원되는 라이브러리를 어떻게 볼 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: asb3993
ms.author: amburns
ms.date: 07/27/2018
ms.openlocfilehash: 7e1017baf7daed68b5e55319a9ce13a4a2df5f2e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61341267"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>PCL에서 지원되는 라이브러리를 어떻게 볼 수 있나요?

- 아래에서 다양 한 PCL 대상 플랫폼을 지 원하는 다양 한 기능 개요를 찾을 수 있습니다 합니다 *지원 되는 기능* 이 페이지의 부분: [https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

- 다른 옵션은 사용 하는 [.NET 이식성 분석기](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) 기존 라이브러리는 PCL 프로필을 변환할 수 있는지 여부를 평가 하 합니다.

- 세 번째 원인은 사용할 수 있는 실제 프로 파일의 내용을 찾아볼 하는 것입니다. 예를 들어 프로필 78를 사용 하 여, 있습니다 수 여기로 이동: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` 및 그 모든 어셈블리를 표시 합니다.

어떤 방법의 선택 하세요 일부 기능은 NuGet 및 Microsoft BCL 라이브러리를 통해 다운로드할 수 있는 참고 합니다.
