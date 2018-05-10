---
title: Xamarin 통합 문서 편집기 바로 가기 키
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 1258a40a527ab47cee78b17454ac818e53c60ad2
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Xamarin 통합 문서 편집기 바로 가기 키

## <a name="the-return-key-and-its-nuances"></a>반환 키와 해당 갖는

다음 표에서 다양 한 키 바인딩 코드를 실행 하 고 마크 다운 작성에 대 한 설명입니다. 이동 된 친숙 하 고 유동적인 키 바인딩이 합리적이 고 일관 된 선택 하려고 합니다.

|키 바인딩을 두|코드 셀|마크 다운 셀|
|--- |--- |--- |
|<kbd>Return</kbd>|<p>셀 버퍼의 끝에 캐럿이 있는 경우 셀이 성공적으로 구문 분석할 수 있습니다 실행 됩니다 및 결과 버퍼를 아래 표시 및 새로운 코드 셀 삽입 됩니다 실행 된 셀 다음 셀에 집중 합니다.</p><p>구문 분석이 실패 하는 경우 버퍼에 새 줄 삽입 됩니다. 구문 분석이 성공 컴파일러 진단 생성 되지 않습니다.</p>|<p><kbd>반환할</kbd> 캐럿 Markdown 컨텍스트에 따라 서로 다른 동작을 수행 합니다.</p><ul><li>캐럿 Markdown 코드 블록에 있으면, 리터럴 새 줄이 삽입 됩니다.</li><li>캐럿 Markdown 목록 블록에 있으면, 새 목록 항목을 만들거나 현재 목록 항목을 분할 합니다.</li><li>캐럿 Markdown 블록의 다른 형식에 있으면, 새 단락 블록을 만들거나 현재 블록을 분할 합니다.</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑Return</kbd></dd><dt>win</dt><dd><kbd>Control‑Return</kbd></dd></dl>|<p>항상 구문 분석 및 셀 내용을 실행 하려고 시도 합니다. 컴파일에 성공한 경우 결과 (실행 예외 포함)는 버퍼를 아래 표시 되 고 후속 셀이 없는 경우 새로 생성 되며 초점을 맞춘 합니다.</p><p>컴파일 오류가 있을 경우 진단 나타납니다 이며 버퍼 변경 하지 않고 캐럿 위치와 포커스가 남아 있습니다.</p>|삽입 하 고 현재 markdown 셀 후 새로운 코드 셀 중점을 둡니다.|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>win</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|삽입 하 고 현재 셀 후 새 markdown 셀 중점을 둡니다.|동일한 동작 <kbd>반환</kbd>|
|<kbd>Shift‑Return</kbd>|항상 캐럿 위치나 내용에 관계 없이 새 줄을 삽입합니다.|현재 마크 다운 블록 내에서 강제 줄 바꿈을 삽입합니다.|
