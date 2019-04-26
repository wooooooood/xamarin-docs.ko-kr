---
title: Xamarin.Mac 응용 프로그램 기본 사항
description: 이 문서는 Xamarin.Mac 응용 프로그램을 개발 하는 경우를 이해 하는 데 필요한 다양 한 개념을 설명 하는 지침에 연결 합니다.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 12/17/2015
ms.openlocfilehash: 376286b73c92cba40de183043b86cb4ffb5e699d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61085307"
---
# <a name="xamarinmac-application-fundamentals"></a>Xamarin.Mac 응용 프로그램 기본 사항

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[공통 패턴 및 관용구](~/mac/app-fundamentals/patterns.md)

전체 C#을 통해 노출 되는 Apple Api를 특정 코드 관용구와 패턴 표시 반복 해 서. Xamarin.iOS 사용 하 여 프로그래밍 경험이 있다면 이러한 익숙해 보일 수 있습니다. 설명서는 자주 참조 이러한 패턴 및 관용구를 반복적으로의 확고 한 이해가 통해 확인할 수 있습니다 설명서의 의미 있도록 합니다.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Mac Api 이해](~/mac/app-fundamentals/mac-apis.md)

대부분의 Xamarin.Mac을 사용 하 여 개발 시간에 대 한 인지, 읽기 및 기본 Objective C Api를 사용 하 여 관계 없이 C#에서 작성 수 있습니다. 그러나 경우에 따라 Apple API 설명서를 읽을 Stack Overflow에서 답변을 솔루션에 문제에 대 한 변환 옮기거나 해야 기존 샘플 비교할 합니다.

## <a name="console-appsmacapp-fundamentalsconsolemd"></a>[콘솔 앱](~/mac/app-fundamentals/console.md)

네이티브 macOS Api에 액세스 하는 "헤드리스" 콘솔 앱을 빌드할 수도 있습니다 Xamarin.Mac을 사용 합니다.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[.Xib 파일 작업](~/mac/app-fundamentals/xib.md)

이 문서를 만들어 Xamarin.Mac 응용 프로그램에 대 한 사용자 인터페이스를 유지 관리 하는 Xcode의 Interface Builder에서 만든.xib 파일을 사용 하 여 작업을 다룹니다.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[사용자 인터페이스 디자인 덜.storyboard/.xib](~/mac/app-fundamentals/xibless-ui.md)

이 문서에서는 C# 코드에서 직접.storyboard 또는.xib 파일을 사용 하 여 Xcode의 Interface Builder를 사용 하지 않고 Xamarin.Mac 응용 프로그램의 사용자 인터페이스를 만들고 있습니다.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[이미지 작업](~/mac/app-fundamentals/image.md)

이 문서에서는 Xamarin.Mac 응용 프로그램의 아이콘 및 이미지를 사용 하 여 작업을 다룹니다. 만들기 설명 하 고 응용 프로그램의 아이콘 및 C# 코드와 Xcode의 Interface Builder에서 이미지를 사용 하 여 만드는 데 필요한 이미지를 유지 관리 합니다.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)

이 문서에서는 키-값 코딩 및 Xcode의 Interface Builder에서 UI 요소에 데이터 바인딩할 수 있도록 관찰 키-값을 사용 하 여 설명 합니다. Xamarin.Mac 응용 프로그램에 대해 작성 해야 하는 C# 코드의 양의 상당히 줄일이 기술을 사용 하 여, 있습니다. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[데이터베이스 작업](~/mac/app-fundamentals/databases.md)

이 문서에서는 키-값 코딩 및 관찰 Xcode의 Interface Builder에서 UI 요소에 SQLite 데이터베이스에 대 한 직접 액세스를 사용 하 여 데이터 바인딩을 허용 하는 키-값을 사용 하 여 설명 합니다. SQLite.NET ORM을 사용 하 여 SQLite 데이터에 액세스할 수 있도록 대해서도 설명 합니다.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[복사 및 붙여넣기 사용](~/mac/app-fundamentals/copy-paste.md)

이 문서는 복사를 제공 하 여 Xamarin.Mac 응용 프로그램에 붙여 임시 보드를 사용 하 여 작업을 다룹니다. 작업 하는 방법을 보여 줍니다 표준 데이터 형식에 게 앱 내에서 사용자 지정 데이터를 지원 하는 방법과 여러 앱 간에 공유할 수 있습니다.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Xamarin.Mac 앱 샌드 박싱](~/mac/app-fundamentals/sandboxing.md)

이 문서에서는 Xamarin.Mac 응용 프로그램 앱 스토어에 릴리스하기 위해 샌드 박싱을 다룹니다. 샌드 박싱에 포함 된 요소의 모든 내용을 다룹니다: 컨테이너 디렉터리, 자격, 사용자가 지정한 사용 권한, 권한 분리 및 커널 적용 합니다.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[AVAudioPlayer로 소리 재생](~/mac/app-fundamentals/sounds.md)

이 아티클에서 AVAudioPlayer를 사용 하 여 소리 재생을 제어 하는 도우미 클래스를 사용 하는 방법에 설명 합니다.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[버그 보고](~/mac/app-fundamentals/troubleshooting.md)

경우에 따라 모든 갇 힐을 원하는 방식으로 작업 하기 위한 API를 가져오려는 또는 버그를 해결 하는 동안 프로젝트에서 작업 하는 동안. Xamarin에서 이러한 목표를 성공적으로 모바일 및 데스크톱 응용 프로그램을 작성 하 되며 몇 가지 리소스를 제공 했습니다.
