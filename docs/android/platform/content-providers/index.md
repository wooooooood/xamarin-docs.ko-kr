---
title: ContentProviders 소개
description: Android 운영 체제는 콘텐츠 공급자를 사용 하 여 미디어 파일, 연락처 및 일정 정보와 같은 공유 데이터에 쉽게 액세스할 수 있습니다. 이 문서에서는 ContentProvider 클래스를 소개 하 고이를 사용 하는 방법에 대 한 두 가지 예를 제공 합니다.
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 3dd321840c4be0729b843897ad51cf5bd2b61196
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758914"
---
# <a name="intro-to-contentproviders"></a>ContentProviders 소개

_Android 운영 체제는 콘텐츠 공급자를 사용 하 여 미디어 파일, 연락처 및 일정 정보와 같은 공유 데이터에 쉽게 액세스할 수 있습니다. 이 문서에서는 ContentProvider 클래스를 소개 하 고이를 사용 하는 방법에 대 한 두 가지 예를 제공 합니다._

## <a name="content-providers-overview"></a>콘텐츠 공급자 개요

*Contentprovider* 는 데이터 리포지토리를 캡슐화 하 고 액세스할 수 있는 API를 제공 합니다. 공급자는 일반적으로 데이터를 표시/관리 하기 위한 UI를 제공 하는 Android 응용 프로그램의 일부로 존재 합니다. 콘텐츠 공급자를 사용 하면 다른 응용 프로그램이 *Contentresolver*라고 하는 공급자 클라이언트 개체를 사용 하 여 캡슐화 된 데이터에 쉽게 액세스할 수 있습니다. 콘텐츠 공급자와 콘텐츠 확인자는 함께 작성 및 사용이 간단 하 고 데이터 액세스를 위한 일관 된 응용 프로그램 간 API를 제공 합니다. 모든 응용 프로그램은를 사용 `ContentProviders` 하 여 데이터를 내부적으로 관리 하 고이를 다른 응용 프로그램에 노출 하도록 선택할 수 있습니다.

응용 `ContentProvider` 프로그램에서 사용자 지정 검색 제안을 제공 하거나 응용 프로그램에서 복잡 한 데이터를 복사 하 여 다른 응용 프로그램에 붙여 넣을 수 있는 기능을 제공 하려는 경우에도이 필요 합니다. 이 문서에서는 xamarin.ios를 사용 하 여 `ContentProviders` 액세스 하 고 빌드하는 방법을 보여 줍니다.

이 섹션의 구조는 다음과 같습니다.

- **작동 방식** &ndash; 에서`ContentProvider` 설계 된 기능과 작동 방법에 대 한 개요입니다.

- **콘텐츠 공급자** 사용 &ndash; 연락처 목록에 액세스 하는 예제입니다.

- **ContentProvider를 사용 하 여 데이터 공유** 동일한 응용 프로그램에서 `ContentProvider` 을 작성 하 고 사용 합니다. &ndash;

`ContentProviders`데이터에 대해 작동 하는 커서는 Listview을 채우는 데 주로 사용 됩니다. 이러한 클래스를 사용 하는 방법에 대 한 자세한 내용은 [listview 및 어댑터 가이드](~/android/user-interface/layouts/list-view/index.md) 를 참조 하세요.

`ContentProviders`Android 또는 기타 응용 프로그램에 의해 노출 되는는 응용 프로그램에 다른 원본의 데이터를 포함 하는 쉬운 방법입니다. 이를 통해 응용 프로그램 내에서 연락처 목록, 사진 또는 일정 이벤트와 같은 데이터에 액세스 하 고 표시할 수 있으며, 사용자가 해당 데이터와 상호 작용할 수 있습니다.

사용자 `ContentProviders` 지정은 자신의 앱 내에서 사용 하거나 다른 응용 프로그램에서 사용할 수 있도록 데이터를 패키지 하는 편리한 방법입니다 (사용자 지정 검색 및 복사/붙여넣기와 같은 특수 한 사용 포함).

이 단원의 항목에서는 코드를 사용 하 고 작성 `ContentProvider` 하는 몇 가지 간단한 예제를 제공 합니다.

## <a name="related-links"></a>관련 링크

- [연락처 Sadapter 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
- [SimpleContentProvider (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
- [콘텐츠 공급자 개발자 가이드](https://developer.android.com/guide/topics/providers/content-providers.html)
- [ContentProvider 클래스 참조](xref:Android.Content.ContentProvider)
- [ContentResolver 클래스 참조](xref:Android.Content.ContentResolver)
- [ListView 클래스 참조](xref:Android.Widget.ListView)
- [CursorAdapter 클래스 참조](xref:Android.Widget.CursorAdapter)
- [UriMatcher 클래스 참조](xref:Android.Content.UriMatcher)
- [Android.Provider](xref:Android.Provider)
- [연락처 Scontract 클래스 참조](xref:Android.Provider.ContactsContract)
