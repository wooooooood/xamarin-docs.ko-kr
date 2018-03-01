---
title: "시기와 방법을 I 파일을 않도록 버그 보고서를?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 3a57c0843a68b454c8cb21c95b280d2731d064cd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>시기와 방법을 I 파일을 않도록 버그 보고서를?


Xamarin의: Bugzilla 버그 추적기 여기에 버그를 파일: [https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all](https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all)합니다.

## <a name="file-a-bug-if"></a>버그를 보고할 경우...


맵을 Xamarin 엔지니어 Xamarin 발생 하는 문제를 재현을 사용 하는 단계 집합이 해야 합니다.

또는

문제에 관련 된 정확한 경우에 따라을 기술 할 수 있습니다 하는 경우에 특히 신중 하 게 문제를 보이는 증상을 기술 할 수 있습니다. <sup> [[1]](#note-1)</sup>


## <a name="best-practices-to-help-xamarin-address-bugs-quickly-and-efficiently"></a>빠르고 효율적으로 Xamarin 주소 버그 수 있도록 유용한 정보


1. <a name="ref-1" />검색 [: Bugzilla](https://bugzilla.xamarin.com/query.cgi?format=specific&amp;bug_status=__all__) 및 버그 보고서 또는 사용량 제안이 직접 문제를 해결할 수 있는 기존 웹.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />에 따라는 [지침을 작성 하는 버그](https://bugzilla.xamarin.com/page.cgi?id=bug-writing.html) 동작을 명확 하 고 가능한 간결 하 게 발생 한 시간 및 된 작업에 대 한 설명을 포함 하 여 예상 대로 문제를 설명 하기 위해 합니다.

1. <a name="ref-3" />모든 관련 스택 추적, 오류 메시지 텍스트를 포함 하거나 충돌 로그. <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />모든 중요 한 나타나는 오류 메시지 스크린샷 첨부 파일에 일반 텍스트로 너무 적어 둡니다.

1. <a name="ref-5" />코드를 가능한 적게 버그를 재현할 수 있는 작은, 자체 포함 된 테스트 사례를 포함 합니다.  새 프로젝트를 (기본 제공 템플릿 중 하나를 사용 하 여 만든) 문제를 재현할 수 없는 문제를 보여 주는 프로젝트를 압축 한 다음 고 버그 보고서에 연결 합니다.  연결 하기 전에 예제 프로젝트 가능한 한 단순한 것을 확인 합니다. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />운영 체제 포함 하 여, 버그가 발견 된 환경을 설명 하 고 [버전의 Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) 및 모든 종속성입니다.

---

## <a name="additional-details"></a>추가 세부 정보

1. <a name="note-1" />[*^*](#ref-1) 이상적으로 "표시 증상"에 대 한 설명을 포함할지는 정보를 충분히 다른 고객에 동일한 문제가 표시 되 고 있는지 여부를 확인할 수 있습니다 (동일한 오류 메시지, 동일한 성능 저하, 충돌이 발생 한에서 같은 스택 추적 _등입니다._ ). "정확한 경우"에 대 한 가지 좋은 예 것 경우 다음과 같이 말할 수 있습니다: "정상적으로 맞출 문제 시간의 75% 이지만 한 가지 사항을 변경 하는 경우 다음 I 문제를 방지할 수 완전히." "정확한 상황"의 다른 비슷한 예는 이전 버전의 Xamarin로의 다운 그레이드 중지 문제입니다.

1. <a name="note-2" />[*^*](#ref-2) 와 마찬가지로, 오류 텍스트 (또는 다른 고유 하 게 설명 텍스트)의 조각은 일반적으로 최상의 검색어입니다. 기존 버그 보고서를 완료 한 다음 모르는 경우 세부 정보를 추가 하거나 새 파일을 더 잘 버그 보고서를 시작 합니다.

1. <a name="note-3" />[*^*](#ref-3) 또 다른 좋은 질문은 모든 Java에 대해 동일한 문제가 보고 된 여부 Objective-c, Swift 응용 프로그램이 나 합니다. 이 경우 Xamarin의 일부가 아니라 가능성이 매우 일부 Android 또는 iOS 자체의 문제가입니다.

1. <a name="note-4" />[*^*](#ref-4) 정보를 포함 하도록의 몇 가지 예:

    1. 에 프로젝트를 빌드할 때 발생 하는 오류 하십시오 포함 되어 전체 [진단 빌드 출력](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) 버그 보고서에 있습니다.
    
    1. 빌드하거나 Visual Studio에서 iOS 프로젝트를 디버깅할 때 발생 하는 오류에 대 한 실행 하십시오 _도움말 > Xamarin > Zip 로그_ 후 오류에 도달 하 고 버그 보고서에서 결과.zip 파일을 포함 합니다.
    
    1. 예외 또는 Android 또는 iOS 앱의 충돌에 대 한 포함 시키세요 관련 "[디버그 Xamarin.Android 및 Xamarin.iOS 앱에 대 한 로그](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)."

1. <a name="note-5" />[*^*](#ref-5) 가능한 경우 특정 문제에 대 한 하나의 뛰어난 옵션은 원래 솔루션에서 새 솔루션에 적은 수의 파일을 추가 하 여 문제가 다시입니다. Xamarin 팀 (재현 단계는 명확 하 게 설명 가정), 테스트 사례를 더 큰 하지만 더 간단한 테스트 사례는 버그를 빠르게 해결할 수는 가장 큰 텍스트에 문제를 조사할 수 경우가 많습니다.


1. <a name="note-6" />[*^*](#ref-6) 이 경우 _하지_ 가능 완전히 새로운 솔루션에 적은 수의 파일을 추가 하 여 문제를 재현 하, 다음 있습니다 수을 압축 하 고 전체 응용 프로그램에 대 한 전체 솔루션 폴더를 연결 합니다. 삭제 하십시오는 `bin`, `obj`, `Components`, 및 `packages` 폴더를 더 작은 파일 zip을 확인 합니다. (IDE 및 빌드 프로세스는 일반적으로 복원 하거나 필요에 따라 이러한 폴더의 콘텐츠를 다시 만듭니다.) 또한으로 생성 되는 솔루션은 여전히 원래 문제를 보여 많은 코드 및 리소스 파일 프로젝트에서 원하는 만큼를 삭제할 수 있습니다.

