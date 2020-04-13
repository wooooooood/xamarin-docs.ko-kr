---
title: 안드로이드 레이아웃 진단 분석기
description: 이 가이드는 Xamarin.Android에서 현재 지원되는 안드로이드 레이아웃 진단 분석기를 모두 나열합니다.
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 04/07/2020
ms.openlocfilehash: 6203ce444bb97fa453a912a724f7f5724558b32a
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987619"
---
# <a name="android-designer-diagnostic-analyzers"></a>안드로이드 디자이너 진단 분석기

이 가이드는 현재 지원되는 모든 Android 레이아웃 진단 분석기를 나열합니다.

## <a name="accessibility"></a>액세스 가능성

다음 분석기는 접근성 지원을 개선하는 데 도움이 됩니다.

| ID | 제목 | 심각도 | Description |
|----|-------|----------|-------------|
| ContentDescription | 없는 이미지`contentDescription` | Warning | 이미지에 누락된 `contentDescription` 특성 |

## <a name="correctness"></a>정확성

다음 분석기는 레이아웃의 정확성 문제를 해결하는 데 도움이 됩니다.

| ID | 제목 | 심각도 | Description | 도움말 |
|----|-------|----------|-------------|------|
| 어댑터뷰어린이 | 어댑터 어린이와 보기 | Warning | 어댑터뷰는 XML에서 자식을 가질 수 없습니다. | [링크](xref:Android.Widget.AdapterView) |
| 누락된 ID | 조각은 `id``tag` | Warning |이 `<fragment>` 태그는 `id` 활동 `tag` 다시 시작 에 걸쳐 상태를 보존 할 또는 을 지정해야합니다 | [링크](xref:Android.App.Fragment) |
| 중첩스크롤수직 | 세로로 스크롤하는 요소 중첩 | Warning | 중첩 스크롤 위젯 |
| 중첩스크롤수평 | 중첩 된 가로 스크롤 요소 | Warning | 중첩 스크롤 위젯 |
| 스크롤 뷰크기 | 스크롤잘못된 fill_parent /match_parent 크기의 어린이보기 | Warning | 스크롤잘못된 fill_parent /match_parent 크기의 어린이보기 |
| 스크롤 뷰 카운트 | ScrollView에는 자식이 하나만 있을 수 있습니다. | Warning | 스크롤 보기에는 자식이 하나만 있을 수 있습니다. |
| 누락 된 안드로이드 네임 스페이스 | 속성에 누락 된 안 드 로이드 네임 스페이스 | Error | 누락 된 안드로이드 XML 네임 스페이스; 귀속은 사용자 지정 특성으로 해석됩니다. |
| 중복 된 아이 들 | 중복 된 아이디 | Error | 단일 레이아웃 내에서 중복 ID |
| 포함레이아웃파라름누락폭및높이높이 | 너비와 높이가 모두 누락되었습니다. | Error | 무시된 레이아웃 매개 변수 포함 | [링크](https://stackoverflow.com/questions/2631614/does-android-xml-layouts-include-tag-really-work) |
| 포함레이아웃파라름누락폭 | 너비가 누락되었습니다. | Error | 무시된 레이아웃 매개 변수 포함 | [링크](https://stackoverflow.com/questions/2631614/does-android-xml-layouts-include-tag-really-work) |
| 포함레이아웃파라름누락높이 | 누락된 높이 | Error | 무시된 레이아웃 매개 변수 포함 | [링크](https://stackoverflow.com/questions/2631614/does-android-xml-layouts-include-tag-really-work) |
| 방향 | 명시적 방향누락 | Error | 명시적 방향누락 |
| 의심스러운 0dp | 의심스러운 0dp 차원 | Error | 의심스러운 0dp 차원 |
| 필수크기폭 | 너비 특성이 누락되었습니다. | Error | 누락된 특성: layout_width |
| 필수크기높이 | 누락된 높이 특성 | Error | 누락된 특성: layout_height |
| 웹뷰레이아웃 | wrap_content 부모의 웹뷰 | Error |
| 잘못된 경우 | 보기 태그에 대 한 잘못 된 경우 | Error | 보기 태그에 대 한 잘못 된 경우 | [링크](xref:Android.App.Fragment) |

## <a name="design"></a>디자인

다음 분석기는 레이아웃 파일에 참여하는 방법을 개선하는 데 도움이 됩니다.

| ID | 제목 | 심각도 | Description |
|----|-------|----------|-------------|
| 하드 코드 색상 | 하드 코딩 된 색상 | 정보 | 하드 코딩된 색상은 종종 불일치로 이어집니다. |
| 하드 코딩 크기  | 하드 코딩 된 크기  | 정보 | 하드 코딩된 크기는 종종 불일치로 이어집니다.  |
| 하드 코딩 텍스트  | 하드 코딩된 텍스트  | Warning | 하드 코딩된 텍스트 |
| 해결되지 않은 리소스 | 확인되지 않은 리소스 URL | Warning | 이 리소스 URL을 확인할 수 없습니다. |
| Xml 오류 | XML 구문 오류 | Error | XML 구문 오류 |

## <a name="performance"></a>성능

다음 분석기는 레이아웃의 성능을 향상시키는 데 도움이 됩니다.

| ID | 제목 | 심각도 | Description |
|----|-------|----------|-------------|
| 중첩 가중치 | 중첩 레이아웃 가중치 | Warning | 중첩 된 가중치는 성능에 좋지 않습니다. |
| 너무많은 전망 | 레이아웃에 보기가 너무 많습니다. | Warning | 레이아웃에 보기가 너무 많습니다. |
| 너무 깊은 레이아웃 | 레이아웃 계층 구조가 너무 깊습니다. | Warning | 레이아웃 계층 구조가 너무 깊습니다. |
| 쓸모없는 부모 | 쓸모없는 부모 레이아웃 | Warning | 쓸모없는 부모 레이아웃 |
| 쓸모없는 잎 | 쓸모없는 잎 레이아웃 | Warning | 이 `%1$s` 보기는 쓸모가 없습니다 `background`(자식 `style`없음, 아니요 , 아니요) `id` |

## <a name="usability"></a>유용성

다음 분석기는 고객의 레이아웃 유용성을 개선하는 데 도움이 됩니다.

| ID | 제목 | 심각도 | Description |
|----|-------|----------|-------------|
| 네거티브 마진 | 마이너스 마진 | Warning | 마이너스 마진 |
| 누락된 입력 유형 | 입력이 없는 편집텍스트Type | Warning | 입력 유형이 지정되지 않음 |
| 입력유형폰 | 편집텍스트가 전화번호로 표시 | Warning | 보기 이름은 전화 번호임을 암시하지만 `phone``inputType` |
| 입력유형번호 | 편집 텍스트가 숫자로 나타납니다. | Warning | 뷰 이름은 숫자이지만 숫자(예: `inputType` `numberDecimal`)를 포함하지 않음을 시사합니다. |
| 입력유형암호 | 편집 텍스트가 암호로 나타납니다. | Warning | 뷰 이름은 이것이 암호임을 `password` `inputType` 암시하지만(예: `textVisiblePassword`) 에 포함되지 않습니다. |
| 입력타이핑핀 | 편집텍스트가 PIN으로 나타납니다. | Warning | 뷰 이름은 이것이 암호(PIN)임을 암시하지만, `numberPassword``inputType` |
| 입력유형이메일 | 편집텍스트가 이메일로 나타납니다. | Warning | 뷰 이름은 전자 메일 주소임을 `email` `inputType` 암시하지만(예: `textEmailAddress`) 에 포함되지 않습니다. |
| 입력타이투리 | 편집 텍스트는 URI로 나타납니다. | Warning | 뷰 이름은 이것이 URI임을 시사하지만 `textUri``inputType` |
| 입력 유형날짜 | 편집 텍스트가 날짜로 나타납니다. | Warning | 뷰 이름은 이것이 날짜임을 `date` `inputType` 시사하지만(예: `datetime`) 에는 포함되지 않습니다. |
