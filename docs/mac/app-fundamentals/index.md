---
title: Xamarin.Mac 응용 프로그램 기본 사항
description: 이 문서는 Xamarin.Mac 응용 프로그램을 개발 하는 경우를 이해 하는 데 필요한 다양 한 개념을 설명 하는 설명서를 링크 합니다.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/17/2015
ms.openlocfilehash: 1c807e97d5218e93c4eb991a9bd80219c9745c2b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791503"
---
# <a name="xamarinmac-application-fundamentals"></a>Xamarin.Mac 응용 프로그램 기본 사항

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[일반적인 패턴 및 관용구](~/mac/app-fundamentals/patterns.md)

C#을 통해 노출 되는 Apple Api 전체에서 특정 관용구와 패턴 대로 반복 됩니다. Xamarin.iOS 사용한 프로그래밍 경험이 있는 경우 이러한 친숙 한 보일 수 있습니다. 설명서 자주 참조 합니다 이러한 패턴 및 관용구를 반복 해 서 그중에서 확실 하 게 이해 하지 찾았으면 설명서의 결과 이해 하면 도움이 됩니다.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Mac Api 이해](~/mac/app-fundamentals/mac-apis.md)

많은 Xamarin.Mac를 사용 하 여 개발 시간이 대 한 생각, 읽기 및 기본 Objective-c Api와 관계 없이 C#에서 작성 수 있습니다. 그러나 때로는 Apple에서 API 설명서를 읽을 귀하의 문제에 대 한 솔루션에 스택 오버플로에서 답변을 변환 옮기거나 해야 기존 샘플을 비교 합니다.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[.Xib 파일 작업](~/mac/app-fundamentals/xib.md)

이 문서에서는.xib 만들고 Xamarin.Mac 응용 프로그램에 대 한 사용자 인터페이스를 유지 관리 하는 Xcode의 인터페이스 작성기에서 만든 파일에서 작업을 설명 합니다.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[사용자 인터페이스 디자인 덜.storyboard/.xib](~/mac/app-fundamentals/xibless-ui.md)

이 문서에서는 C# 코드에서 직접.storyboard 또는.xib 파일로 Xcode의 인터페이스 작성기를 사용 하지 않고 Xamarin.Mac 응용 프로그램의 사용자 인터페이스를 생성 합니다.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[이미지 작업](~/mac/app-fundamentals/image.md)

이 문서에서는 Xamarin.Mac 응용 프로그램의 아이콘 이미지와 작업을 수행 합니다. 만들기 설명 하 고 응용 프로그램의 아이콘 및 C# 코드와 Xcode의 인터페이스 작성기에서 이미지를 사용 하 여 만드는 데 필요한 이미지를 유지 관리 합니다.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)

이 문서에서는 키-값 코딩 및 관찰 Xcode의 인터페이스 작성기에서 UI 요소에 대 한 데이터 바인딩을 허용 하는 키-값을 사용 하 여 설명 합니다. Xamarin.Mac 응용 프로그램에 써야 하는 C# 코드의 양의 상당히 줄일이 방법을 사용할 경우 있습니다. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[데이터베이스 작업](~/mac/app-fundamentals/databases.md)

이 문서에서는 키-값 코딩 및 관찰 Xcode의 인터페이스 작성기에서 UI 요소에 SQLite 데이터베이스에 직접 액세스할 수 있는 데이터 바인딩을 허용 하는 키-값을 사용 하 여 설명 합니다. SQLite 데이터에 액세스할 수 있도록 SQLite.NET ORM을 사용 하 여 대해서도 설명 합니다.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[복사 및 붙여넣기 작업](~/mac/app-fundamentals/copy-paste.md)

이 문서에 작업 복사본을 제공 하 여 Xamarin.Mac 응용 프로그램에서 여 붙여 넣는 지에 설명 합니다. 작업 하는 방법을 보여 줍니다 제공 앱 내에서 사용자 지정 데이터를 지 원하는 방법 및 여러 앱 간에 공유할 수 있는 표준 데이터 형식입니다.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[샌드 박싱 Xamarin.Mac 응용 프로그램](~/mac/app-fundamentals/sandboxing.md)

이 문서에서는 샌드 박싱 앱 스토어에서 릴리스에 대 한 Xamarin.Mac 응용 프로그램에 설명 합니다. 모든 샌드 박싱 속하게 될 요소를 다룹니다: 컨테이너 디렉터리, 권한 부여, 사용자가 지정한 사용 권한, 권한 분할 및 커널 적용 합니다.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[AVAudioPlayer와 소리 재생](~/mac/app-fundamentals/sounds.md)

이 문서는 AVAudioPlayer를 사용 하 여 소리 재생을 제어 하는 도우미 클래스를 사용 하는 방법을 보여 줍니다.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[버그를 보고](~/mac/app-fundamentals/troubleshooting.md)

경우에 따라 원하는 방식으로 작동 하기 위한 API를 가져올 수 없어서 또는 버그를 해결 하는 프로젝트에서 작업 하는 동안 갇 우리는 모두 합니다. Xamarin에서 목표는 모바일 및 데스크톱 응용 프로그램 작성 성공 수 및 몇 가지 리소스를 제공 했습니다.
