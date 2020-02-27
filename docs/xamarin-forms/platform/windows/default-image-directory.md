---
title: Windows의 기본 이미지 디렉터리
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 이미지 자산이 로드 될 프로젝트의 디렉터리를 정의 하는 Windows 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 537A032B-74DD-4D43-864E-7D7113286D0D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: 52197b980726936f4368ef1e4507ea671c9e70b1
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646661"
---
# <a name="default-image-directory-on-windows"></a>Windows의 기본 이미지 디렉터리

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 유니버설 Windows 플랫폼 플랫폼에 맞게 이미지 자산이 로드 되는 프로젝트의 디렉터리를 정의 합니다. 이미지 자산을 포함 하는 프로젝트 디렉터리를 나타내는 `string` `Application.ImageDirectory`을 설정 하 여 XAML에서 사용 됩니다.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
             ...
             windows:Application.ImageDirectory="Assets">
    ...
</Application>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
Application.Current.On<Windows>().SetImageDirectory("Assets");
```

`Application.On<Windows>` 메서드는이 플랫폼별가 유니버설 Windows 플랫폼 에서만 실행 되도록 지정 합니다. [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스의 `Application.SetImageDirectory` 메서드는 이미지가 로드 되는 프로젝트 디렉터리를 지정 하는 데 사용 됩니다. 또한 `GetImageDirectory` 메서드를 사용 하 여 응용 프로그램 이미지 자산을 포함 하는 프로젝트 디렉터리를 나타내는 `string`를 반환할 수 있습니다.

그러면 응용 프로그램에 사용 되는 모든 이미지가 지정 된 프로젝트 디렉터리에서 로드 됩니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
