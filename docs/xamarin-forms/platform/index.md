---
title: Xamarin.Forms 플랫폼 기능
description: 이 가이드는 플랫폼별 기능들을 Xamarin.Forms 앱에서 다양한 기술을 사용하여 활용할 수 있는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2018
ms.openlocfilehash: 9bac53f71178ac321dea162d346295556a8f7adb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998757"
---
# <a name="xamarinforms-platform-features"></a>Xamarin.Forms 플랫폼 기능

Xamarin.Forms는 확장이 용이하고,  [Effects](~/xamarin-forms/app-fundamentals/effects/index.md), [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md) 등을 사용하여 플랫폼별 기능들을 통합할 수 있습니다.

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

이 가이드에서는 기존 Xamarin.Forms Android 앱을 업데이트하여 머티리얼 디자인을 구현하는 방법을 설명합니다.

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[응용 프로그램 인덱싱 및 딥 링크 설정](deep-linking.md)

앱 인덱싱은 몇 번 사용 후 잊어버려질 것을 검색 결과에 표시합니다. 딥 링크는 앱이 검색 기록에 앱 정보를 포함하고 있을 때 응답할 수 있도록 도와줍니다. 이는 주로 딥 링크로부터 참조된 페이지로 이동할 때입니다.

## <a name="device-classdevicemd"></a>[장치 클래스](device.md)

`Device` 클래스로 플랫폼에 따른 행동을 공유된 코드에서 구현하는 방법과 XAML을 사용한 유저 인터페이스를 만드는 방법을 소개합니다. 또한, 백그라운드 스레드에서 UI 컨트롤을 수정할 때 중요한 `BeginInvokeOnMainThread`에 대해서도 다룹니다.

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

**Info.plist** 와 `UIAppearance` API로 일부의 iOS 스타일링을 수행할 수 있습니다. 이 가이드는 Core Spotlight 검색 기능을 포함하여 iOS 앱인 Xamarin.Forms 솔루션에 iOS 9의 기능들을 어떻게 추가하는지에 대한 예시를 포함하고 있습니다.

## <a name="gtkgtkmd"></a>[GTK](gtk.md)

Xamarin.Forms는 GTK# 앱에 대한 제한적인 지원을 하고 있습니다.

## <a name="macmacmd"></a>[Mac](mac.md)

Xamarin.Forms는 macOS 앱에 대한 제한적인 지원을 하고 있습니다.

## <a name="native-formsnative-formsmd"></a>[네이티브 양식](native-forms.md)

Native Forms는 Xamarin.Forms의 파생된 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 페이지들을 네이티브 Xamarin.iOS, Xamarin.Android 및 유니버설 Windows 플랫폼(UWP) 프로젝트에서 사용할 수 있게 파생합니다.

## <a name="native-viewsnative-viewsindexmd"></a>[네이티브 뷰](native-views/index.md)

Xamarin.Forms에서 iOS, Android 및 유니버설 Windows 플랫폼의 네이티브 뷰를 직접 참조할 수 있습니다. 속성과 이벤트 핸들러는 네이티브 뷰에서 설정할 수 있으며, Xamarin.Forms 뷰와 상호작용할 수 있습니다.

## <a name="platform-specificsplatform-specificsindexmd"></a>[Platform-Specifics](platform-specifics/index.md)

Platform-Specifics는 특정 플랫폼에서만 가능한 기능들을 Custom renderer 또는 Effects 등을 사용하지 않고도 구현할 수 있게 합니다.

## <a name="pluginspluginsmd"></a>[플러그인](plugins.md)

Xamarin.Forms 앱을 확장할 수 있도록 도와주는 다양한 오픈소스 플러그인들을 Github, Nuget 및 Xamarin Component Store에서 찾을 수 있습니다.

## <a name="tizentizenmd"></a>[Tizen](tizen.md)

Tizen.NET을 사용하면 .NET 앱을 Xamarin.Forms 및 Tizen .NET Framework를 사용하여 빌드할 수 있습니다.

## <a name="windowswindowsindexmd"></a>[Windows](windows/index.md)

Xamarin.Forms는 Windows 10에서 유니버설 Windows 플랫폼(UWP)을 지원합니다. 이 문서는 기존의 Xamarin.Forms 솔루션에 UWP 프로젝트를 어떻게 추가하는지 설명합니다.

## <a name="wpfwpfmd"></a>[WPF](wpf.md)

Xamarin.Forms는 Windows Presentation Foundation(WPF) 앱에 대한 제한적인 지원을 하고 있습니다.
