---
title: Xamarin 통합 문서 편집기 바로 가기 키
description: 이 문서에서는 Xamarin Workbooks 편집기에서 사용할 수 있는 바로 가기 키를 설명 합니다. 특히 반환 키를 사용 하는 다양 한 방법에 살펴봅니다.
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 87af9f824117b20250c02a3e070652607626de44
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61341052"
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Xamarin 통합 문서 편집기 바로 가기 키

## <a name="the-return-key-and-its-nuances"></a>반환 키와 해당 미묘한 차이

다음 표에서 코드를 실행 하 고 markdown 작성에 대 한 다양 한 키 바인딩을 설명 합니다. 친숙 하 고 유동적인 키 바인딩을 적절 하 고 일관 된 선택 주의 이동 합니다.

|키 바인딩|코드 셀|Markdown 셀|
|--- |--- |--- |
|<kbd>Return</kbd>|<p>셀 버퍼의 끝에 캐럿이 있는 경우 셀이 성공적으로 구문 분석할 수 있습니다 실행 된다는 것 버퍼 아래 결과 표시 됩니다 고 새 코드 셀을 삽입 될 셀 실행 셀의 다음 셀에 집중 합니다.</p><p>구문 분석에 실패할 경우 새 줄 버퍼에 삽입 됩니다. 구문 분석이 성공 컴파일러 진단 생성 되지 않습니다.</p>|<p><kbd>반환</kbd> 캐럿에서 Markdown 컨텍스트에 따라 다른 동작이 나타납니다.</p><ul><li>캐럿 Markdown 코드 블록에 리터럴 새 줄이 삽입 됩니다.</li><li>캐럿 Markdown 목록 블록에 있으면 새 목록 항목을 만들거나 현재 목록 항목을 분할 합니다.</li><li>캐럿 Markdown 블록의 다른 모든 형식에 새 단락 블록을 만들거나 현재 블록을 분할 합니다.</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑Return</kbd></dd><dt>Win</dt><dd><kbd>Control‑Return</kbd></dd></dl>|<p>항상 구문 분석 하 고 셀의 내용을 실행 하려고 시도 합니다. 컴파일 성공 하면 결과 실행 예외 등 버퍼 아래 표시 됩니다 및 후속 셀이 없는 경우 새로 생성 되며 집중.</p><p>모든 컴파일 오류가 있는 경우 진단 표시 되 고 변경 하지 않고 캐럿 위치를 사용 하 여 버퍼 포커스가 유지 됩니다.</p>|삽입 하 고 현재 markdown 셀 후 새 코드 셀을 중점을 둡니다.|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>Win</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|삽입 하 고 현재 셀 후 새 markdown 셀 중점을 둡니다.|동일한 동작 <kbd>반환</kbd>|
|<kbd>Shift‑Return</kbd>|항상 콘텐츠 또는 캐럿 위치에 관계 없이 새 줄을 삽입 합니다.|현재 Markdown 블록 내에서 하드 줄 바꿈을 삽입 합니다.|
