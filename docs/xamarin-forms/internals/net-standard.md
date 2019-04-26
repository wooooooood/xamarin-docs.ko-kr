---
title: Xamarin.Forms에서.NET standard 2.0 지원
description: 이 문서에서는.NET Standard 2.0을 사용 하 여 Xamarin.Forms 응용 프로그램을 변환 하는 방법을 설명 합니다. .NET standard는 모든.NET 구현에서 사용할 수 있는.NET Api 사양입니다.
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: c3e46592bb8760ff85eaeb5dce119897a97dfe89
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293288"
---
# <a name="net-standard-20-support-in-xamarinforms"></a>Xamarin.Forms에서.NET standard 2.0 지원

_이 문서에서는.NET Standard 2.0을 사용 하 여 Xamarin.Forms 응용 프로그램을 변환 하는 방법을 설명 합니다._

.NET standard는 모든.NET 구현에서 사용할 수 있는.NET Api 사양입니다. 쉽게 데스크톱 응용 프로그램, 모바일 앱 및 게임 간에 코드를 공유 하 여 클라우드 서비스, 서로 다른 플랫폼에 동일한 Api를 가져와서 합니다. .NET 표준에서 지원 되는 플랫폼에 대 한 자세한 내용은 참조 하세요. [.NET 구현 지원](/dotnet/standard/net-standard#net-implementation-support)합니다.

.NET standard 라이브러리는 이식 가능한 클래스 라이브러리 (PCL)에 대 한 대체 합니다. 그러나.NET Standard를 대상으로 하는 라이브러리 PCL을 그대로 및.NET 표준 기반 PCL을 이라고 합니다. 특정 PCL 프로필.NET 표준 버전에 매핑되고 매핑되는 프로필의 경우 두 라이브러리 형식을 서로 참조 하는 일을 할 수 있습니다. 자세한 내용은 [PCL 호환성](/dotnet/standard/net-standard#pcl-compatibility)합니다.

2.4 Xamarin.Forms PCL을.NET Standard 2.0 라이브러리를 사용 하 여 대체 하 여 Xamarin.Forms 응용 프로그램은.NET Standard 2.0을 대상으로 지정할 수 있습니다. 이 다음과 같이 수행할 수 있습니다.

- 확인 [.NET Core 2.0](https://www.microsoft.com/net/download/core) 설치 됩니다.
- 2.4를 크거나 Xamarin.Forms를 사용 하는 Xamarin.Forms 솔루션을 업데이트 합니다.
- .NET Standard 라이브러리를.NET Standard 2.0을 대상으로 하는 솔루션에 추가 합니다.
- .NET Standard 라이브러리에 추가 되는 클래스를 삭제 합니다.
- .NET Standard 라이브러리에 Xamarin.Forms 2.4 이상 NuGet 패키지를 추가 합니다.
- 플랫폼 프로젝트에서.NET Standard 라이브러리에 대 한 참조를 추가 하 고 Xamarin.Forms 사용자 인터페이스 논리를 포함 하는 PCL 프로젝트에 대 한 참조를 제거 합니다.
- .NET 표준 라이브러리 PCL 프로젝트에서 파일을 복사 합니다.
- Xamarin.Forms 사용자 인터페이스 논리를 포함 하는 PCL 프로젝트를 제거 합니다.


## <a name="related-links"></a>관련 링크

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md)
