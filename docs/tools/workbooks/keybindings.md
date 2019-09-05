---
title: Xamarin Workbooks 편집기 바로 가기 키
description: 이 문서에서는 Xamarin Workbooks 편집기에서 사용할 수 있는 바로 가기 키에 대해 설명 합니다. 특히 반환 키를 사용 하는 여러 가지 방법을 살펴봅니다.
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: 9904d0f9fb1acfc3c3c197b9881c2add00aba534
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285322"
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Xamarin Workbooks 편집기 바로 가기 키

## <a name="the-return-key-and-its-nuances"></a>반환 키 및 해당 미묘한 차이

다음 표에서는 코드를 실행 하 고 markdown를 작성 하는 다양 한 키 바인딩을 설명 합니다. 친숙 하 고 유체 인 적절 하 고 일관 된 키 바인딩을 선택 하는 것은 매우 신중 하 게 수행 되었습니다.

|키 바인딩|코드 셀|Markdown 셀|
|--- |--- |--- |
|<kbd>Return</kbd>|<p>캐럿이 셀 버퍼의 끝에 있고 셀이 성공적으로 구문 분석 될 수 있는 경우 실행 되 고 결과가 버퍼 아래에 표시 되며, 실행 된 셀 뒤에 새 코드 셀이 삽입 되 고 포커스가 있는 셀 다음에 표시 됩니다.</p><p>구문 분석이 실패 하면 새 줄이 버퍼에 삽입 됩니다. 구문 분석이 실패 하면 컴파일러 진단이 생성 되지 않습니다.</p>|<p><kbd>Return</kbd> 은 캐럿의 Markdown 컨텍스트에 따라 다른 동작을 보여 주는 것입니다.</p><ul><li>캐럿이 Markdown 코드 블록에 있으면 리터럴 새 줄이 삽입 됩니다.</li><li>캐럿이 Markdown list 블록에 있으면 새 목록 항목을 만들거나 현재 목록 항목을 분할 합니다.</li><li>캐럿이 다른 형식의 Markdown 블록에 있으면 새 단락 블록을 만들거나 현재 블록을 분할 합니다.</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>명령 반환</kbd></dd><dt>Win</dt><dd><kbd>컨트롤 반환</kbd></dd></dl>|<p>항상 셀 내용의 구문 분석 및 실행을 시도 합니다. 컴파일이 성공적으로 완료 되 면 결과 (실행 예외 포함)가 버퍼 아래에 표시 되 고, 후속 셀이 없으면 새 항목을 만들고 포커스가 표시 됩니다.</p><p>컴파일 오류가 있는 경우 진단이 표시 되 고 버퍼는 캐럿 위치가 변경 되지 않은 상태로 계속 집중 됩니다.</p>|현재 markdown 셀 뒤에 새 코드 셀을 삽입 하 고 포커스를 합니다.|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>Win</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|현재 셀 뒤에 새 markdown 셀을 삽입 하 고 포커스를 합니다.|<kbd>Return</kbd> 과 동일한 동작|
|<kbd>Shift‑Return</kbd>|캐럿 위치 또는 내용에 관계 없이 항상 새 줄을 삽입 합니다.|현재 Markdown 블록 내에 하드 줄 바꿈을 삽입 합니다.|
