---
title: 사용 가능한 어셈블리
description: 이 문서에서는 Xamarin.ios, Xamarin Android 및 Xamarin.ios에서 사용할 수 있는 어셈블리를 나열 합니다. .NET Standard 라이브러리 및 이식 가능한 클래스 라이브러리에 대 한 설명서 링크도 제공 됩니다.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: 238011b4762f2d394629e75fbde476e618219df2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016358"
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
> |Assembly|API 호환성|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N|CJK, MidEast, 기타, 희귀, 서 부 포함|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|SQLite 용 ADO.NET 공급자 제한 사항을 참조 하세요.|✓|✓|✓|
> |Mono.Data.Tds.dll|TDS 프로토콜 지원 [system.object](xref:System.Data)내의 [system.object](xref:System.Data.SqlClient) 지원에 사용 됩니다.|✓|✓|✓|
> |Mono. 동적. &#8203;인터프리터 .dll| |✓| | |
> |Mono.Security.dll|암호화 Api.|✓|✓|✓|
> |monotouch.dll|이 어셈블리에는 C# CocoaTouch API에 대 한 바인딩이 포함 되어 있습니다. 클래식 iOS 프로젝트 내 에서만 사용할 수 있습니다.|✓| | |
> |Monotouch.dialog. &#8203;Dialog-1| |✓| | |
> |Monotouch.dialog. &#8203;Nreceived lite .dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|IPhone 장치 지원을 제공 하도록 확장 된 OpenGL/OpenAL 개체 지향 Api입니다.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)및 다음 네임 스페이스의 형식:<br />System.Collections.Specialized<br />컴퓨터. &#8203;System.componentmodel<br />System.componentmodel<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />시스템 .Net 캐시<br />System.Net.Mail<br />시스템 .Net Mime<br />System.Net. &#8203;System.net.networkinformation<br />System.Net.Security<br />System.Net.Sockets<br />System.object. &#8203;T e m<br />System.Runtime.Versioning<br />System.web. &#8203;Accesscontrol-namespace<br />System.Security.Authentication<br />System.web. &#8203;암호화<br />System.Security.Permissions<br />System.Threading<br />시스템 타이머|✓|✓|✓|
> |컴퓨터. &#8203;System.componentmodel. &#8203;컴퍼지션 .dll| |✓|✓|✓|
> |컴퓨터. &#8203;System.componentmodel. &#8203;Dataannotations .dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx) . [일부 기능이 제거](~/ios/data-cloud/system.data.md)되었습니다.|✓|✓|✓|
> |System.object. &#8203;서비스. &#8203;클라이언트 .dll|전체 oData 클라이언트.|✓|✓|✓|
> |System.IO. &#8203;압축| |✓|✓|✓|
> |System.IO. &#8203;압축. &#8203;파일 시스템| |✓|✓|✓|
> |System.Json.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net. &#8203;Http.sys| |✓|✓|✓|
> |컴퓨터. &#8203;숫자 .dll| |✓|✓|✓|
> |System.object. &#8203;Serialization .dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |컴퓨터. &#8203;ServiceModel .dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx) 에 있는 WCF stack|✓|✓|✓|
> |컴퓨터. &#8203;ServiceModel. &#8203;내부 .dll| |✓|✓|✓|
> |컴퓨터. &#8203;ServiceModel. &#8203;웹 .dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)및 다음 네임 스페이스의 형식: <br />시스템<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |컴퓨터. &#8203;트랜잭션도 .dll|[.Net 3.5](https://msdn.microsoft.com/library/ms229335.aspx); [시스템 데이터](~/ios/data-cloud/system.data.md) 지원의 일부입니다.|✓|✓|✓|
> |System.web. &#8203;서비스 .dll|서버 기능이 제거 된 .NET 3.5 프로필의 기본 웹 서비스입니다.|✓|✓|✓|
> |컴퓨터. &#8203;Windows .dll| |✓|✓|✓|
> |컴퓨터. &#8203;Xml .dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.xml. &#8203;Linq .dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|이 어셈블리에는 C# CocoaTouch API에 대 한 바인딩이 포함 되어 있습니다. 이는 통합 iOS 프로젝트 에서만 사용 됩니다.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono. Android. &#8203;내보내기 .dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |컴퓨터. &#8203;System.enterpriseservices| | |✓| |
> |Xamarin.ios. &#8203;Nreceived lite .dll| | |✓| |
> |System.runtime.compilerservices. &#8203;기호 작성기 .dll|컴파일러 작성자.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |컴퓨터. &#8203;Drawing .dll|System.object는 Xamarin.ios, .NET 4.5 또는 모바일 프레임 워크에 대 한 Unified API에서 지원 되지 않습니다. [Sysdrawing-coregraphics](https://github.com/mono/sysdrawing-coregraphics) 라이브러리를 사용 하 여 IOS 및 macos에 Drawing 지원을 추가할 수 있습니다.|✓| |✓|
