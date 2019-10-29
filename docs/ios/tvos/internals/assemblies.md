---
title: Xamarin for tvOS에서 지 원하는 어셈블리
description: TvOS 응용 프로그램에 사용할 수 있는 기능을 명확 하 게 설명 하기 위해이 문서에서는 tvOS 개발용 Xamarin에서 지 원하는 어셈블리의 목록을 제공 합니다.
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/07/2016
ms.openlocfilehash: 371440f2e1ab28e802bf2d184b3e17d073a0c774
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030681"
---
# <a name="assemblies-supported-by-xamarin-for-tvos"></a>Xamarin for tvOS에서 지 원하는 어셈블리

## <a name="supported-assemblies"></a>지원 되는 어셈블리

Xamarin에서 tvOS 앱에 대해 지 원하는 어셈블리의 목록입니다. 이에 대 한 자세한 목록은 아래에 나열 되어 있습니다.  몇 가지 주목할 만한 생략에는 `System.EnterpriseServices`ASP.NET stack 및 Windows가 포함 됩니다.

|Assembly|강화|API 호환성|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|컴파일러 작성자.|
|Mono.Data.Sqlite.dll|1.2|SQLite 용 ADO.NET 공급자 [제한 사항](~/ios/data-cloud/system.data.md)을 참조 하세요.|
|Mono.Data.Tds.dll|1.2|TDS 프로토콜 지원 [system.object](~/ios/data-cloud/system.data.md)내의 [system.object](xref:System.Data.SqlClient) 지원에 사용 됩니다.|
|Mono.Security.dll|1.0|암호화 Api.|
|monotouch.dll|1.0|이 어셈블리에는 [ C# CocoaTouch API에 대 한 바인딩이](https://docs.microsoft.com/dotnet/api/?view=xamarinios-10.8)포함 되어 있습니다.|
|mscorlib.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|[IPhone 장치 지원을 제공 하도록 확장](xref:OpenGLES)된 OpenGL/openal 개체 지향 api입니다.|
|System.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)및 다음 네임 스페이스의 형식: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.componentmodel</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>시스템 .Net 캐시</li> <li>System.Net.Mail</li> <li>시스템 .Net Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>시스템 타이머</li></ul>|
|System.Core.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx). [일부 기능이 제거](~/ios/data-cloud/system.data.md)되었습니다.|
|System.object.|에서처럼|전체 oData 클라이언트.|
|System.Drawing|1.0|System.object-Classic API 그리기<br />_System.object는 Xamarin.ios .NET 4.5 또는 모바일 프레임 워크에 대 한 Unified API에서 지원 되지 않습니다._|
|System.Json.dll|1.1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx) 에 있는 [WCF](https://docs.microsoft.com/xamarin/cross-platform/data-cloud/web-services/) stack|
|System.ServiceModel.Web.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)및 다음 네임 스페이스의 형식: <ul><li>시스템</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx); [시스템 데이터](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) 지원의 일부입니다.|
|System.Web.Services|1.1|서버 기능이 제거 된 .NET 3.5 프로필의 [기본 웹 서비스](https://docs.microsoft.com/xamarin/cross-platform/data-cloud/web-services/) 입니다.|
|System.Xml.dll|1.0|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>이식 가능한 클래스 라이브러리

TvOS는 Mac 바인딩 외에도 [.Net 이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)를 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
