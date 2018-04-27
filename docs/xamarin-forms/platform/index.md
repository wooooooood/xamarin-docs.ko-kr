---
title: 플랫폼 기능
description: Xamarin.forms는 플랫폼 특정 기능 활용
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/20/2017
ms.openlocfilehash: be131bdbfeceabd72494708cdfe9a263da9bbbd8
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="platform-features"></a>플랫폼 기능

Xamarin.Forms는 확장 가능 하 고 사용 하 여 플랫폼 특정 기능을 통합 하면 [효과](~/xamarin-forms/app-fundamentals/effects/index.md), [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)는 [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md), 등입니다.

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

이 가이드에서는 기존 Xamarin.Forms Android 앱을 업데이트 하 여 자료 디자인을 구현 하는 방법을 설명 합니다.

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[응용 프로그램 인덱싱 및 딥 링크 설정](deep-linking.md)

응용 프로그램 인덱싱을 그렇지 않은 경우 무시 될 후 몇 가지 사용 하 여 검색 결과에 표시 하 여 관련 상태를 유지 하는 응용 프로그램 수 있습니다. 직접 링크 수 딥 링크에서 참조 하는 페이지로 이동 하 여 일반적으로 응용 프로그램 데이터를 포함 하는 검색 결과에 응답할 수 있습니다.

## <a name="device-classdevicemd"></a>[장치 클래스](device.md)

사용 하는 방법의 `Device` 클래스 공유 코드 (XAML을 사용 하 여 포함) 사용자 인터페이스에서 플랫폼 특정 동작을 만들 수 있습니다. 에 대해서도 설명 `BeginInvokeOnMainThread` 백그라운드 스레드에서 UI 컨트롤을 수정 하는 경우 필수적입니다.

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

일부 iOS 스타일 지정을 통해 수행할 수 **Info.plist** 및 `UIAppearance` API입니다. 이 가이드에서는 iOS 앱의 핵심 스포트라이트 검색을 포함 하는 Xamarin.Forms 솔루션에 iOS 9 기능을 포함 하는 방법의 예입니다.

## <a name="gtkgtkmd"></a>[GTK](gtk.md)

Xamarin.Forms는 GTK # 응용 프로그램에 대 한 미리 보기 지원이 되었습니다.

## <a name="macmacmd"></a>[Mac](mac.md)

Xamarin.Forms는 macOS 앱에 대 한 미리 보기 지원이 되었습니다.

## <a name="wpfwpfmd"></a>[WPF](wpf.md)

Xamarin.Forms에는 Windows Presentation Foundation (WPF) 응용 프로그램에 대 한 미리 보기 지원이 되었습니다.

## <a name="native-formsnative-formsmd"></a>[네이티브 양식](native-forms.md)

Xamarin.Forms를 허용 하는 기본 형식 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-네이티브 Xamarin.iOS, Xamarin.Android, 및 유니버설 Windows 플랫폼 (UWP) 프로젝트에서 사용할 수 있도록 페이지를 파생 합니다.

## <a name="native-viewsnative-viewsindexmd"></a>[네이티브 뷰](native-views/index.md)

Xamarin.Forms에서 iOS, Android 및 유니버설 Windows 플랫폼에서 네이티브 뷰를 직접 참조할 수 있습니다. 기본 보기에서 속성 및 이벤트 처리기를 설정할 수 있습니다 및 Xamarin.Forms 보기와 상호 작용할 수 있습니다.

## <a name="platform-specificsplatform-specificsindexmd"></a>[플랫폼 특성](platform-specifics/index.md)

플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 없이 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

## <a name="pluginspluginsmd"></a>[플러그 인](plugins.md)

Github, Nuget을 및 Xamarin.Forms 앱을 늘리기 위해 Xamarin 구성 요소 저장소에서 사용 가능한 다양 한 오픈 소스 플러그 인 있습니다.

## <a name="windowswindowsindexmd"></a>[Windows](windows/index.md)

Xamarin.Forms는 Windows 10에서 유니버설 Windows 플랫폼 (UWP)에 대 한 지원 합니다. 이 문서에 추가 하는 방법에 설명 된 UWP 프로젝트를 기존 Xamarin.Forms 솔루션입니다.
