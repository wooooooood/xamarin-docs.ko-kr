---
title: 사용 가능한 어셈블리
description: 이 문서는 Xamarin.iOS, Xamarin.Android, 및 Xamarin.Mac에 사용할 수 있는 어셈블리를 나열합니다. .NET 표준 라이브러리 및 이식 가능한 클래스 라이브러리에 대 한 설명서에도 연결합니다.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: b73a818d3864c7c4d1d776e104d95090e87f5877
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781909"
---
# <a name="available-assemblies"></a>사용 가능한 어셈블리

Xamarin.iOS, Xamarin.Android, 및 Xamarin.Mac까지 여러 어셈블리에 모두 제공 합니다. Silverlight는 데스크톱.NET 어셈블리의는 확장 된 하위 집합, 처럼 Xamarin 플랫폼의 몇 가지 Silverlight 및 데스크톱.NET 어셈블리는 확장 된 하위 집합 이기도 합니다.

Xamarin 플랫폼 ABI 다른 프로필에 대 한 컴파일된 기존 어셈블리와 호환 되지 않습니다. (처럼 별도로 Silverlight 및.NET 3.5를 대상으로 소스 코드를 다시 컴파일할 필요)에서 올바른 프로 파일을 대상으로 하는 어셈블리를 생성 하도록 소스 코드를 컴파일해야 합니다.

세 가지 모드로 Xamarin.Mac 응용 프로그램을 컴파일할 수 있습니다: 모바일 프로필 큐 레이트의 Xamarin을 사용 하 여 하나, Xamarin.Mac.NET 4.5 Framework 수 있는 대상으로 기존 전체 데스크톱 어셈블리 및.NET API를 사용 하는 지원 되지 않는 항목 모노 시스템에서 찾을 설치 합니다. 자세한 내용은 참조 하십시오 우리의 [대상 프레임 워크](~/mac/platform/target-framework.md) 설명서입니다.


## <a name="net-standard-libraries"></a>.NET 표준 라이브러리

IOS, Android 및 Mac 뿐만 아니라 Xamarin 프로젝트에서 사용할 수 있는 바인딩 [.NET 표준 라이브러리](~/cross-platform/app-fundamentals/net-standard.md)합니다.

## <a name="portable-class-libraries"></a>이식 가능한 클래스 라이브러리
 
Xamarin 프로젝트도 사용할 수 있는 [.NET 이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)않더라도이 기술은 표준.NET 지지 않는 것입니다.

## <a name="supported-assemblies"></a>지원 되는 어셈블리

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|API 호환성|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|서쪽 CJK, MidEast, 드문, 기타 포함|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|SQLite;에 대 한 ADO.NET 공급자 제한을 참조 하십시오.|✓|✓|✓|
> |Mono.Data.Tds.dll|TDS 프로토콜을 지원 합니다. 에 사용 되는 [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) 내에서 지원 되 [System.Data](https://developer.xamarin.com/api/namespace/System.Data/)합니다.|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|암호화 Api입니다.|✓|✓|✓|
> |monotouch.dll|이 어셈블리에는 C# 바인딩 CocoaTouch API에 포함 되어 있습니다. 이것이 기본 iOS 프로젝트 내에서 사용할 수만 있습니다.|✓| | |
> |MonoTouch 합니다. &#8203;1.dll 대화 상자| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|OpenGL/OpenAL 개체 지향 Api, iPhone 장치를 지원 하도록 확장 합니다.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), 다음 네임 스페이스의 형식 및:<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security 합니다. &#8203;암호화<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx) 와 [일부 기능이 제거](~/ios/data-cloud/system.data.md)합니다.|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|전체 oData 클라이언트입니다.|✓|✓|✓|
> |System.IO.&#8203;Compression| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System.&#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|에 있는 WCF 스택에 [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), 다음 네임 스페이스의 형식 및: <br />시스템<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |시스템입니다. &#8203;Transactions.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx);의 일부로 [System.Data](~/ios/data-cloud/system.data.md) 지원 합니다.|✓|✓|✓|
> |System.Web.&#8203;Services.dll|제거 된 서버 기능 사용 하 여.NET 3.5 프로필에서 기본 웹 서비스입니다.|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|이 어셈블리에는 C# 바인딩 CocoaTouch API에 포함 되어 있습니다. 이 통합 iOS 프로젝트에만 사용 됩니다.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|에 대 한 컴파일러 작성기입니다.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |시스템입니다. &#8203;Drawing.dll|System.Drawing API-클래식 API만 합니다. System.Drawing은 Xamarin.Mac.NET 4.5 또는 모바일 프레임 워크에 대 한 통합 API에서 지원 되지 않습니다. IOS 및 OS X를 사용 하 여 System.Drawing 지원을 추가할 수 있습니다는 [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics) 라이브러리|✓| |✓|
