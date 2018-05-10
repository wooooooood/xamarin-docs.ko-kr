---
title: Nuget 패키지를 업데이트 한 후 누락 된 패키지 오류
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: asb3993
ms.author: amburns
ms.openlocfilehash: 4f1c4f51b690e35711efc0fc4fac56565b9cb3c7
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Nuget 패키지를 업데이트 한 후 누락 된 패키지 오류

Xamarin.Forms 샘플 응용 프로그램 솔루션에이 문제는 보고 주로 하지만이 문제에 대 한 잠재적인 NuGet 패키지를 사용 하는 프로젝트에서 발생할 수 있습니다. 

프로젝트 또는 솔루션에서 Nuget 패키지를 업데이트 후 이전 패키지 버전 번호와 같은 참조 하는 오류가 나타납니다.

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.  
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)

```

이 예에서 *Xamarin.Forms.1.3.1.6296* Nuget 패키지 업데이트와 함께 제거 된 이전 버전 번호입니다.

이전 패키지 버전 번호를 참조 하는 XML 요소에.csproj 파일을 수동으로 추가 해야 하는 경우 발생할 수 있습니다 또는 편집 Nuget은 제거 하거나 되었다면 수동으로 추가/편집 되므로 프로젝트는 이제 된 패키지에 대 한 계획 업데이트 삭제 합니다. 

이 문제를 해결 하려면 수동으로.csproj 파일을 편집 및 이전 버전 번호를 참조 하는 요소를 모두 삭제 합니다. 

샘플 요소 (이전 패키지 버전 번호가 서로) 하는 경우 제거 하려면:

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />

```

