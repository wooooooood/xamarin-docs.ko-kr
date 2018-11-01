---
title: Nuget 패키지를 업데이트한 후 패키지 오류 누락
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 7cb802dd60d4e4879a260ff56d4f94ea5acb2965
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674849"
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Nuget 패키지를 업데이트한 후 패키지 오류 누락

이 문제는 주로 Xamarin.Forms 샘플 앱 솔루션에 보고 되었습니다 하지만 NuGet 패키지를 사용 하는 모든 프로젝트에서이 문제에 대 한 잠재적인 발생할 수 있습니다. 

프로젝트 또는 솔루션의 Nuget 패키지를 업데이트 뒤와 같은 이전 패키지 버전 번호를 참조 하는 오류가 표시:

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.  
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)
```

이 예제의 *Xamarin.Forms.1.3.1.6296* Nuget 패키지 업데이트를 사용 하 여 제거 된 이전 버전 번호입니다.

이전 패키지 버전 번호를 참조 하는.csproj 파일에서 XML 요소를 수동으로 추가 된 경우 발생할 수 있습니다 또는 편집, Nuget은 제거 하거나 되었다면 수동으로 추가/편집 된 패키지를 프로젝트는 이제 찾고 있도록 업데이트 삭제 합니다. 

수동으로이 문제를 해결 하려면.csproj 파일을 편집 및 모든 이전 버전 번호를 참조 하는 요소를 삭제 합니다. 

샘플 요소 (경우 이전 패키지 버전 번호)을 제거 하려면:

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />
```