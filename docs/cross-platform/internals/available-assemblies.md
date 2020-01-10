---
title: 사용 가능한 어셈블리
description: 이 문서에서는 Xamarin.ios, Xamarin Android 및 Xamarin.ios에서 사용할 수 있는 어셈블리를 나열 합니다. .NET Standard 라이브러리 및 이식 가능한 클래스 라이브러리에 대 한 설명서 링크도 제공 됩니다.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: 31066d09b1e753dd054a6a908b626ca3edee008e
ms.sourcegitcommit: 04929b5ff4384ca807727bec7c0467111a7eb283
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75867610"
---
# <a name="available-assemblies"></a>사용 가능한 어셈블리

Xamarin.ios, Xamarin Android 및 Xamarin.ios는 모두 수십 개 이상의 어셈블리와 함께 제공 됩니다. Silverlight가 데스크톱 .NET 어셈블리의 확장 된 하위 집합인 것 처럼 Xamarin 플랫폼도 여러 Silverlight 및 데스크톱 .NET 어셈블리의 확장 된 하위 집합입니다.

Xamarin 플랫폼은 다른 프로필에 대해 컴파일된 기존 어셈블리와 ABI와 호환 되지 않습니다. 소스 코드를 다시 컴파일하여 올바른 프로필을 대상으로 하는 어셈블리를 생성 해야 합니다 (Silverlight 및 .NET 3.5을 별도로 대상으로 하는 소스 코드를 다시 컴파일해야 하는 것 처럼).

Xamarin.ios 응용 프로그램은 세 가지 모드에서 컴파일할 수 있습니다. 하나는 Xamarin의 큐 레이트 모바일 프로필을 사용 하 고, Xamarin.ios .NET 4.5 프레임 워크를 사용 하 여 기존 전체 데스크톱 어셈블리를 대상으로 하 고, 시스템 Mono에서 발견 된 .NET API를 사용 하는 지원 되지 않는 것입니다. 설치가. 자세한 내용은 [대상 프레임 워크](~/mac/platform/target-framework.md) 설명서를 참조 하세요.

## <a name="net-standard-libraries"></a>.NET Standard 라이브러리

IOS, Android 및 Mac 바인딩 외에 Xamarin 프로젝트는 [.NET Standard 라이브러리](~/cross-platform/app-fundamentals/net-standard.md)를 사용할 수 있습니다.

## <a name="portable-class-libraries"></a>이식 가능한 클래스 라이브러리

Xamarin 프로젝트는 [.Net 이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)를 사용할 수도 있습니다 .이 기술은 .NET Standard를 위해 더 이상 사용 되지 않습니다.

## <a name="supported-assemblies"></a>지원 되는 어셈블리

**참조 관리자 > 어셈블리 > Framework** (Visual Studio 2017) 및 **편집 참조 > 패키지** (Mac용 Visual Studio)와 Xamarin 플랫폼과의 호환성을 참조 관리자에서 사용할 수 있는 어셈블리입니다.

> [!div class="mx-tdCol2BreakAll"]
> |어셈블리|API 호환성|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |l18N.dll|CJK, MidEast, 기타, 희귀, 서 부 포함|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |Microsoft.CSharp.dll| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |Mono.CSharp.dll| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |Mono.Data.Sqlite.dll|SQLite 용 ADO.NET 공급자 제한 사항을 참조 하세요.|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |Mono.Data.Tds.dll|TDS 프로토콜 지원 [system.object](xref:System.Data)내의 [system.object](xref:System.Data.SqlClient) 지원에 사용 됩니다.|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |Mono.Dynamic.&#8203;Interpreter.dll| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")| | |
> |Mono.Security.dll|암호화 Api.|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |monotouch.dll|이 어셈블리에는 C# CocoaTouch API에 대 한 바인딩이 포함 되어 있습니다. 클래식 iOS 프로젝트 내 에서만 사용할 수 있습니다.|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")| | |
> |Monotouch.dialog. &#8203;Dialog-1| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")| | |
> |MonoTouch.&#8203;NUnitLite.dll| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |OpenTK-1.0.dll|IPhone 장치 지원을 제공 하도록 확장 된 OpenGL/OpenAL 개체 지향 Api입니다.|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)및 다음 네임 스페이스의 형식:<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.web. &#8203;암호화<br />System.Security.Permissions<br />System.Threading<br />System.Timers|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.Data.dll|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx) . [일부 기능이 제거](~/ios/data-cloud/system.data.md)되었습니다.|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.Data.&#8203;Services.&#8203;Client.dll|전체 oData 클라이언트.|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.IO.&#8203;Compression| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.Json.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.Net.&#8203;Http.dll| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.&#8203;Numerics.dll| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.&#8203;ServiceModel.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx) 에 있는 WCF stack|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)및 다음 네임 스페이스의 형식: <br />System<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |컴퓨터. &#8203;트랜잭션도 .dll|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx); [시스템 데이터](~/ios/data-cloud/system.data.md) 지원의 일부입니다.|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.Web.&#8203;Services.dll|서버 기능이 제거 된 .NET 3.5 프로필의 기본 웹 서비스입니다.|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.&#8203;Windows.dll| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.&#8203;Xml.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |System.Xml.Serialization.dll| |![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |Xamarin.iOS.dll|이 어셈블리에는 C# CocoaTouch API에 대 한 바인딩이 포함 되어 있습니다. 이는 통합 iOS 프로젝트 에서만 사용 됩니다.|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")| | |
> |Java.Interop.dll| | |![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")| |
> |Mono.Android.dll| | |![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")| |
> |Mono.Android.&#8203;Export.dll| | |![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")| |
> |Mono.Posix.dll| | |![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")| |
> |System.&#8203;EnterpriseServices.dll| | |![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|컴파일러 작성자.| | |![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |Xamarin.Mac.dll| | | |![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|
> |컴퓨터. &#8203;Drawing .dll|System.object는 Xamarin.ios, .NET 4.5 또는 모바일 프레임 워크에 대 한 Unified API에서 지원 되지 않습니다. [Sysdrawing-coregraphics](https://github.com/mono/sysdrawing-coregraphics) 라이브러리를 사용 하 여 IOS 및 macos에 Drawing 지원을 추가할 수 있습니다.|![Xamarin.ios 지원 됨](~/media/shared/yes.png "Xamarin.ios 지원 됨")| |![Xamarin.ios 지원](~/media/shared/yes.png "Xamarin.ios 지원")|