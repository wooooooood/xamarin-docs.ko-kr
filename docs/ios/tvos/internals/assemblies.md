---
title: TvOS 용 Xamarin에서 지원 되는 어셈블리
description: TvOS 응용 프로그램에 제공 되는 기능을 명확히 하기 위해이 문서에서는 tvOS 개발용 Xamarin에서 지원 되는 어셈블리의 목록을 제공 합니다.
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: df50b4280335001f2d27ff23a91e4098eed3ba99
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870211"
---
# <a name="assemblies-supported-by-xamarin-for-tvos"></a>TvOS 용 Xamarin에서 지원 되는 어셈블리

## <a name="supported-assemblies"></a>지원 되는 어셈블리

Xamarin.tvOS 앱 용 Xamarin에서 지 원하는 어셈블리의 목록입니다. 이러한 자세한 목록 아래에 나열 됩니다.  몇 가지 주목할 만한 누락 포함 `System.EnterpriseServices`, ASP.NET 스택 및 Windows.Forms 합니다.

|Assembly|추가됨|API 호환성|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|컴파일러 작성자입니다.|
|Mono.Data.Sqlite.dll|1.2|SQLite; 용 ADO.NET 공급자 참조 [제한 사항](~/ios/data-cloud/system.data.md)합니다.|
|Mono.Data.Tds.dll|1.2|TDS 프로토콜 지원. 에 사용 되는 [System.Data.SqlClient](xref:System.Data.SqlClient) 내에서 지 원하는 [System.Data](~/ios/data-cloud/system.data.md)합니다.|
|Mono.Security.dll|1.0|암호화 Api입니다.|
|monotouch.dll|1.0|이 어셈블리는 포함 된 [CocoaTouch API에 C# 바인딩을](https://docs.microsoft.com/dotnet/api/?view=xamarinios-10.8)합니다.|
|mscorlib.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|OpenGL/OpenAL 개체 지향 Api [iPhone 장치 지원을 제공 하기 위해 확장](xref:OpenGLES)합니다.|
|System.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), 다음 네임 스페이스의 형식 및: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)하십시오 [일부 기능이 제거를 사용 하 여](~/ios/data-cloud/system.data.md)입니다.|
|System.Data.Service.Client.dll|3.x|전체 oData 클라이언트입니다.|
|System.Drawing|1.0|System.Drawing API-클래식 API만 합니다.<br />_System.Drawing은 Xamarin.Mac.NET 4.5 또는 모바일 프레임 워크에 대 한 통합 API에서 지원 되지 않습니다._|
|System.Json.dll|1.1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) 에 있는 스택 [Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), 다음 네임 스페이스의 형식 및: <ul><li>시스템</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx);의 일부로 [System.Data](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) 지원 합니다.|
|System.Web.Services|1.1|[기본 웹 서비스](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) 제거 된 서버 기능을 사용 하 여.NET 3.5 프로필에서.|
|System.Xml.dll|1.0|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>이식 가능한 클래스 라이브러리

Xamarin.tvOS Mac 바인딩 외에도 사용할 수 있습니다 [.NET 이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)합니다.

## <a name="related-links"></a>관련 링크

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
