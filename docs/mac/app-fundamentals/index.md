---
title: Xamarin.ios 응용 프로그램 기본 사항
description: 이 문서는 Xamarin.ios 응용 프로그램을 개발할 때 이해 하는 데 필요한 다양 한 개념을 설명 하는 가이드로 연결 됩니다.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 12/17/2015
ms.openlocfilehash: 2603360162ee9918e83b9f5c74b8086f71d02df8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030095"
---
# <a name="xamarinmac-application-fundamentals"></a>Xamarin.ios 응용 프로그램 기본 사항

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[일반 패턴 및 관용구](~/mac/app-fundamentals/patterns.md)

를 통해 C#노출 되는 Apple api 전체에서 특정 관용구 패턴은 다시 발생 합니다. Xamarin.ios를 사용 하 여 프로그래밍 하는 경험이 있다면 이러한 작업을 잘 살펴볼 수 있습니다. 설명서는 종종 이러한 패턴을 반복적으로 참조 하 고 관용구을 확실 하 게 이해 하 게 되 면 찾은 설명서를 이해 하는 데 도움이 됩니다.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Mac Api 이해](~/mac/app-fundamentals/mac-apis.md)

Xamarin.ios를 사용 하 여 개발 하는 데 많은 시간이 소요 되는 경우 기본 목표-C Api C# 에 크게 신경 쓰지 않고, 읽기 및 쓰기를 수행할 수 있습니다. 그러나 경우에 따라 Apple에서 API 설명서를 읽고 문제에 대 한 Stack Overflow의 답변을 솔루션으로 변환 하거나 기존 샘플과 비교 해야 합니다.

## <a name="console-appsmacapp-fundamentalsconsolemd"></a>[콘솔 앱](~/mac/app-fundamentals/console.md)

Xamarin.ios를 사용 하 여 기본 macOS Api에 액세스 하는 "헤드리스" 콘솔 앱을 빌드할 수도 있습니다.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[Xib 파일 작업](~/mac/app-fundamentals/xib.md)

이 문서에서는 Xcode의 Interface Builder에서 만든 xib 파일을 사용 하 여 Xamarin.ios 응용 프로그램에 대 한 사용자 인터페이스를 만들고 유지 관리 하는 방법을 설명 합니다.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[. storyboard/. xib less 사용자 인터페이스 디자인](~/mac/app-fundamentals/xibless-ui.md)

이 문서에서는 Xcode의 Interface Builder를 storyboard 또는 xib 파일과 함께 사용 하지 C# 않고 코드에서 직접 xamarin.ios 응용 프로그램의 사용자 인터페이스를 만드는 방법을 설명 합니다.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[이미지 작업](~/mac/app-fundamentals/image.md)

이 문서에서는 Xamarin.ios 응용 프로그램에서 이미지 및 아이콘 작업을 설명 합니다. 응용 프로그램의 아이콘을 만드는 데 필요한 이미지를 만들고 유지 관리 하는 데 필요한 이미지를 C# 만들고 유지 관리 하는 데 필요한 이미지 및 코드와 Xcode의 Interface Builder

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)

이 문서에서는 Xcode의 Interface Builder에서 UI 요소에 대 한 데이터 바인딩을 허용 하기 위해 키-값 코딩 및 키-값 관찰을 사용 하는 방법을 설명 합니다. 이 기술을 사용 하면 Xamarin.ios 응용 프로그램에 대해 작성 C# 해야 하는 코드의 양을 크게 줄일 수 있습니다. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[데이터베이스 작업](~/mac/app-fundamentals/databases.md)

이 문서에서는 키-값 코딩 및 키-값 관찰을 사용 하 여 Xcode의 Interface Builder의 UI 요소에 SQLite 데이터베이스에 직접 액세스 하는 데이터 바인딩을 허용 하는 방법을 설명 합니다. SQLite.NET ORM을 사용 하 여 SQLite 데이터에 대 한 액세스를 제공 하는 방법도 다룹니다.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[복사 및 붙여넣기 작업](~/mac/app-fundamentals/copy-paste.md)

이 문서에서는 Xamarin.ios 응용 프로그램에서 복사 및 붙여넣기 기능을 제공 하기 위해 대지의 작업을 설명 합니다. 여러 앱 간에 공유할 수 있는 표준 데이터 형식으로 작업 하는 방법과 앱에서 제공 하는 사용자 지정 데이터를 지 원하는 방법을 보여 줍니다.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Xamarin.ios 앱 샌드 박싱](~/mac/app-fundamentals/sandboxing.md)

이 문서에서는 앱 스토어에서 릴리스에 대 한 Xamarin.ios 응용 프로그램을 샌드 박싱 하는 방법에 대해 설명 합니다. 샌드 박싱으로 이동 하는 모든 요소에 대해 설명 합니다. 컨테이너 디렉터리, 자격, 사용자 결정 권한, 권한 분리 및 커널 적용입니다.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[Av오디오 플레이어로 소리 재생](~/mac/app-fundamentals/sounds.md)

이 문서에서는 도우미 클래스를 사용 하 여 Av오디오 플레이어를 사용 하는 소리의 재생을 제어 하는 방법을 보여 줍니다.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[버그 보고](~/mac/app-fundamentals/troubleshooting.md)

때로는 API를 사용 하 여 원하는 방식으로 또는 버그를 해결할 수 없는 경우에도 프로젝트에서 작업 하는 동안 문제가 발생할 수 있습니다. Xamarin의 목표는 모바일 및 데스크톱 응용 프로그램을 작성 하는 데 도움이 되는 것 이며, 도움이 되는 몇 가지 리소스를 제공 했습니다.
