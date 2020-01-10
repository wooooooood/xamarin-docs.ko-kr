---
title: NuGet 패키지를 업데이트 한 후 누락 된 패키지 오류
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: a1ed4a2be63973e9e26dcb06163a3f9fd7eae649
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728098"
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>NuGet 패키지를 업데이트 한 후 누락 된 패키지 오류

이 문제는 주로 Xamarin.ios 샘플 앱 솔루션에 대해 보고 되었지만 NuGet 패키지를 사용 하는 모든 프로젝트에서이 문제가 발생할 수 있습니다.

프로젝트나 솔루션에서 NuGet 패키지를 업데이트 한 후에는 다음과 같은 이전 패키지 버전 번호를 참조 하는 오류가 표시 됩니다.

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)
```

이 예제에서 *1.3.1.6296* 는 NuGet 패키지 업데이트를 사용 하 여 제거 된 이전 버전 번호입니다.

이는 이전 패키지 버전 번호를 참조 하는 .csproj 파일의 XML 요소를 수동으로 추가 하거나 편집 하는 경우에 발생할 수 있습니다. 또한 수동으로 추가/편집한 경우 NuGet은 제거 하거나 업데이트 하지 않으므로 프로젝트에서 삭제 된 패키지를 찾습니다.

이 문제를 해결 하려면 .csproj 파일을 수동으로 편집 하 고 이전 버전 번호를 참조 하는 모든 요소를 삭제 합니다.

제거할 샘플 요소 (이전 패키지 버전 번호가 있는 경우):

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />
```
