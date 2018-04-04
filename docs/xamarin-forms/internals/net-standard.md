---
title: Xamarin.Forms에.NET 표준 2.0 지원
description: 이 문서에서는 표준.NET 2.0을 사용 하는 Xamarin.Forms 응용 프로그램을 변환 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 8685f1e10b5094e6f58e8efea51e6dd216dfa000
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="net-standard-20-support-in-xamarinforms"></a>Xamarin.Forms에.NET 표준 2.0 지원

_이 문서에서는 표준.NET 2.0을 사용 하는 Xamarin.Forms 응용 프로그램을 변환 하는 방법을 설명 합니다._

표준.NET은 모든.NET 구현에서 사용 가능 하도록 설계 된.NET Api에 대 한 사양입니다. 예제에서는 데스크톱 응용 프로그램, 모바일 앱 및 게임에서 코드를 공유 및 다른 플랫폼에 동일한 Api를 전환 하 여 클라우드 서비스를 보다 쉽게. 표준.NET에서 지 원하는 플랫폼에 대 한 정보를 참조 하십시오. [.NET 구현 지원](/dotnet/standard/net-standard#net-implementation-support/)합니다.

.NET 표준 라이브러리는 대체에 대 한 PCL 이식 가능한 클래스 라이브러리 ()입니다. 그러나 표준.NET을 대상으로 하는 라이브러리 여전히 PCL, 하며 표준.NET 기반 PCL으로 참조 됩니다. .NET 표준 버전에 특정 PCL 프로필 매핑되고 매핑되는 프로필에 대해 두 개의 라이브러리 형식 서로 참조 하는 일을 할 수 있습니다. 자세한 내용은 참조 [PCL 호환성](/dotnet/standard/net-standard#pcl-compatibility)합니다.

Xamarin.Forms 2.4 PCL는 표준.NET 2.0 라이브러리와 대체 하 여 Xamarin.Forms 응용 프로그램을 표준.NET 2.0을 대상으로 지정할 수 있습니다. 다음과 같이 수행할 수 있습니다.

- 확인 [.NET Core 2.0](https://www.microsoft.com/net/download/core) 가 설치 되어 있습니다.
- 2.4, 이상 Xamarin.Forms를 사용 하는 Xamarin.Forms 솔루션을 업데이트 합니다.
- 표준.NET 2.0을 대상으로 하는 솔루션에는 표준.NET 라이브러리를 추가 합니다.
- .NET 표준 라이브러리에 추가 하는 클래스를 삭제 합니다.
- Xamarin.Forms 2.4 (또는 그 이상) NuGet 패키지를 표준.NET 라이브러리를 추가 합니다.
- 플랫폼 프로젝트에서.NET 표준 라이브러리에 대 한 참조를 추가 하 고 Xamarin.Forms 사용자 인터페이스 논리를 포함 하는 PCL 프로젝트에 대 한 참조를 제거 합니다.
- 표준.NET 라이브러리를 PCL 프로젝트에서 파일을 복사 합니다.
- Xamarin.Forms 사용자 인터페이스 논리를 포함 하는 PCL 프로젝트를 제거 합니다.


## <a name="related-links"></a>관련 링크

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md)
