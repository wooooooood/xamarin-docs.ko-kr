---
title: Xamarin.Android Errors Matrix
ms.topic: article
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1c9284f3db1c503b5c88f0d310f916f4c9e3363d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin.Android Errors Matrix

## <a name="errors-reference"></a>오류 참조

이 문서 Xamarin에서 다양 한 오류 코드에 대 한 정보를 제공합니다.

<table>
    <thead>
        <tr>
            <td>범주</td>
            <td>설명</td>
        </tr>
    </thead>
    <tbody>
        <tr><td>XA0xxx</td><td><code>mandroid</code> 오류</td></tr>
        <tr><td>XA1xxx</td><td>파일을 복사 / symlink (프로젝트와 관련 된) 오류</td></tr>
        <tr><td>XA2xxx</td><td>링커 오류</td></tr>
        <tr><td>XA3xxx</td><td>AOT 오류</td></tr>
        <tr><td>XA4xxx</td><td>코드 생성 오류</td></tr>
        <tr><td>XA5xxx</td><td>GCC 및 도구 체인 오류</td></tr>
        <tr><td>XA6xxx</td><td><code>mandroid</code> 내부 도구 오류</td></tr>
        <tr><td>XA7xxx</td><td>예약됨</td></tr>
        <tr><td>XA8xxx</td><td>예약됨</td></tr>
        <tr><td>XA9xxx</td><td>라이선스 오류</td></tr>
    </tbody>
</table>

## <a name="error-codes"></a>오류 코드

### <a name="xa0xxx-errors"></a>XA0xxx 오류

<table>
    <thead>
        <tr><td>오류 코드</td><td>설명</td></tr>
    </thead>
    <tbody>
        <tr><td>XA0000</td><td>예기치 않은 오류-에 버그 보고서를 채우십시오. <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA0001</td><td>'-devname 장치별 개입 없이 제공 된</td></tr>
        <tr><td>XA0002</td><td>환경 변수 ' {'이 (가) 구문 분석할 수 없습니다.</td></tr>
        <tr><td>XA0003</td><td>응용 프로그램 이름 '{0}.exe' SDK 또는 제품 어셈블리 (.dll) 이름과 충돌 합니다.</td></tr>
        <tr><td>XA0004</td><td>새로운 refcounting 논리 sgen도 사용으로 설정 하려면 필요 합니다.</td></tr>
        <tr><td>XA0005</td><td>출력 디렉터리 ' {'이 (가) 존재 하지 않습니다.</td></tr>
        <tr><td>XA0006</td><td>' {0} 에' 개발자 플랫폼에 없는 경우 사용 하 여-플랫폼 SDK를 지정 하는 구획 =</td></tr>
        <tr><td>XA0007</td><td>루트 어셈블리 ' {'이 (가) 존재 하지 않습니다.</td></tr>
        <tr><td>XA0008</td><td>하나의 루트 어셈블리를 제공 해야</td></tr>
        <tr><td>XA0009</td><td>어셈블리를 로드 하는 동안 오류가 발생 했습니다: {0}</td></tr>
        <tr><td>XA0010</td><td>명령줄 인수 구문 분석 하지 못했습니다: {0}</td></tr>
        <tr><td>XA0011</td><td>{0}가 MonoTouch 지 원하는 것 보다 더 최신 런타임 ({1 \})에 대해 빌드된</td></tr>
        <tr><td>XA0012</td><td>불완전 한 데이터가을 완료할 제공 `{0}`합니다.</td></tr>
        <tr><td>XA0013</td><td>Sgen 너무 사용 하도록 설정할 필요 프로 파일링 지원</td></tr>
        <tr><td>XA0014</td><td>iOS {0} ARMv6를 대상으로 하는 응용 프로그램 빌드를 지원 하지 않습니다.</td></tr>
        <tr><td>XA0020</td><td>Mandroid 경로 확인할 수 없습니다.</td></tr>
        <tr><td>XA0100</td><td>EmbeddedNativeLibrary ' {'이 (0) Android 응용 프로그램 프로젝트에서 유효 하지 않습니다. AndroidNativeLibrary를 대신 사용 하십시오.</td></tr>
    </tbody>
</table>

### <a name="xa1xxx-errors"></a>XA1xxx 오류

<table>
    <thead>
        <tr><td>오류 코드</td><td>설명</td></tr>
    </thead>
    <tbody>
        <tr><td>XA1001</td><td>지정된 된 디렉터리에 응용 프로그램을 찾을 수 없습니다.</td></tr>
        <tr><td>XA1002</td><td>복사 된 파일, symlink를 만들 수 없습니다.</td></tr>
        <tr><td>XA1003</td><td>' {'이 (0) 응용 프로그램을 종료할 수 없습니다. 응용 프로그램을 수동으로 중지 해야 할 수 있습니다.</td></tr>
        <tr><td>XA1004</td><td>설치 된 응용 프로그램의 목록을 가져올 수 없습니다.</td></tr>
        <tr><td>XA1005</td><td>장치 '{1 \}'에서 ' {'이 (0) 응용 프로그램을 종료 하지 못했습니다: {2 \}입니다. 응용 프로그램을 수동으로 중지 해야 할 수 있습니다.</td></tr>
        <tr><td>XA1006</td><td>장치 '{1 \}'에 ' {'이 (0) 응용 프로그램을 설치 하지 못했습니다: {2 \}입니다.</td></tr>
        <tr><td>XA1007</td><td>장치 '{1 \}'에서 ' {'이 (0)는 응용 프로그램을 시작 하지 못했습니다: {2 \}입니다. 여전히 응용 프로그램을 탭 하 여 수동으로 시작할 수 있습니다.</td></tr>
        <tr><td>XA1008</td><td>시뮬레이터를 시작 하지 못했습니다: {0}</td></tr>
        <tr><td>XA1009</td><td>어셈블리 ' {'이 (가) '{1 \}'에 복사할 수 없습니다: {2 \}</td></tr>
        <tr><td>XA1010</td><td>어셈블리 ' {'이 (가)를 로드할 수 없습니다: {1 \}</td></tr>
        <tr><td>XA1011</td><td>누락 된 리소스 파일을 추가할 수 없습니다: ' {'이 (0)</td></tr>
        <tr><td>XA1101</td><td>응용 프로그램을 시작할 수 없습니다.</td></tr>
        <tr><td>XA1102</td><td>응용 프로그램에서 (중지)에 연결 하지 못했습니다: {0}</td></tr>
        <tr><td>XA1103</td><td>분리할 수 없습니다.</td></tr>
        <tr><td>XA1104</td><td>패킷을 보내지 못했습니다: {0}</td></tr>
        <tr><td>XA1105</td><td>예기치 않은 응답 유형</td></tr>
        <tr><td>XA1106</td><td>장치에서 응용 프로그램의 목록을 가져올 수 없습니다: 요청 시간이 초과 되었습니다.</td></tr>
        <tr><td>XA1107</td><td>응용 프로그램을 시작 하지 못했습니다.</td></tr>
        <tr><td>XA1201</td><td>시뮬레이터를 로드 하지 못했습니다: {0}</td></tr>
        <tr><td>XA1301</td><td>네이티브 라이브러리 `{0}` ({1 \})는 현재 빌드 architecture(s) ({2 \}) 일치 하지 않기 때문에 무시 되었습니다</td></tr>
    </tbody>
</table>

### <a name="xa2xxx-errors"></a>XA2xxx 오류

<table>
    <thead>
        <tr><td>오류 코드</td><td>설명</td></tr>
    </thead>
    <tbody>
        <tr><td>XA2001</td><td>어셈블리를 연결할 수 없습니다.</td></tr>
        <tr><td>XA2002</td><td>참조를 확인할 수 없습니다: {0}</td></tr>
        <tr><td>XA2003</td><td>링크는 사용 되지 옵션 ' {'이 (가) 무시 됩니다.</td></tr>
        <tr><td>XA2004</td><td>추가 링커 정의 파일 ' {'이 (가) 찾을 수 없습니다.</td></tr>
        <tr><td>XA2005</td><td>' {'이 (0)에서 정의 구문 분석할 수 없습니다.</td></tr>
        <tr><td>XA2006</td><td>'{2 \}'에서 메타 데이터 항목 ' {'이 (가) ('{1 \}'에 정의 됨)에 대 한 참조를 확인할 수 없습니다.</td></tr>
    </tbody>
</table>

### <a name="xa3xxx-errors"></a>XA3xxx 오류

이들은 AOT 오류입니다.

<table>
    <thead>
        <tr><td>오류 코드</td><td>설명</td></tr>
    </thead>
    <tbody>
        <tr><td>XA3001</td><td>어셈블리 ' {'이 (가) AOT을 하지 수 없습니다.</td></tr>
        <tr><td>XA3002</td><td>AOT 제한: [MonoPInvokeCallback]으로 데코레이팅되 어 있으므로 메서드 ' {'이 (가) 정적 이어야 합니다.</td></tr>
        <tr><td>XA3003</td><td>충돌 하는-디버그 및-원하는 llvm 옵션입니다. 소프트 디버깅 사용할 수 없습니다.</td></tr>
    </tbody>
</table>

### <a name="xa4xxx-errors"></a>XA4xxx 오류

이들은 코드 생성 오류입니다.

<table>
    <thead>
        <tr><td>오류 코드</td><td>설명</td></tr>
    </thead>
    <tbody>
        <tr><td>XA4001</td><td>기본 서식 파일을 expansed 수 없는 `{0}`합니다.</td></tr>
        <tr><td>XA4101</td><td>등록자 형식에 대 한 시그니처를 작성할 수 없습니다 `{0}`합니다.</td></tr>
        <tr><td>XA4102</td><td>등록자에 잘못 된 형식이 발견 `{0}` 메서드 서명에서 `{2}`합니다. 대신 `{1}`를 사용하세요.</td></tr>
        <tr><td>XA4103</td><td>등록자에 잘못 된 형식이 발견 `{0}` 메서드 서명에서 `{2}`: 유형을 INativeObject을를 구현 하지만 두 개를 사용 하는 생성자가 없습니다 (IntPtr, bool) 인수</td></tr>
        <tr><td>XA4104</td><td>등록자 형식에 대 한 반환 값을 마샬링할 수 `{0}` 메서드 서명에서 `{1}`합니다.</td></tr>
        <tr><td>XA4105</td><td>등록자 형식의 매개 변수를 마샬링할 수 `{0}` 메서드 서명에서 `{1}`합니다.</td></tr>
        <tr><td>XA4106</td><td>등록자 구조에 대 한 반환 값을 마샬링할 수 `{0}` 메서드 서명에서 `{1}`합니다.</td></tr>
        <tr><td>XA4107</td><td>등록자 형식의 매개 변수를 마샬링할 수 `{0}` 메서드 서명에서 `{1}`합니다.</td></tr>
        <tr><td>XA4108</td><td>등록 기관에는 관리 되는 형식에 대 한 ObjectiveC 형식을 가져올 수 없습니다 `{0}`합니다.</td></tr>
        <tr><td>XA4109</td><td>생성 된 등록자 코드를 컴파일하지 못했습니다. 에 버그 보고서를 제출 하세요 <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA4110</td><td>등록자 형식의 out 매개 변수를 마샬링할 수 `{0}` 메서드 서명에서 `{1}`합니다.</td></tr>
        <tr><td>XA4111</td><td>등록자 형식에 대 한 시그니처를 작성할 수 없습니다 `{0}' in method `{1 \} '.</td></tr>
        <tr><td>XA4200</td><td>'Claas' 형식에 대 한 ACW의만 생성할 수 있습니다.</td></tr>
        <tr><td>XA4201</td><td>{0} 형식의 JNI 이름을 확인할 수 없습니다.</td></tr>
        <tr><td>XA4203</td><td>지정된 된 형식 이름을 정규화 해야 합니다.</td></tr>
        <tr><td>XA4204</td><td>인터페이스 형식 ' {'이 (가) 확인할 수 없습니다. 어셈블리 참조가 있는지 확인하세요.</td></tr>
        <tr><td>XA4205</td><td>[ExportField] 메서드 매개 변수가 0에 사용할 수 있습니다.</td></tr>
        <tr><td>XA4206</td><td>[내보내기] 제네릭 형식에 사용할 수 없습니다.</td></tr>
        <tr><td>XA4207</td><td>[ExportField] 제네릭 형식에 사용할 수 없습니다.</td></tr>
        <tr><td>XA4208</td><td>[Java.Interop.ExportFieldAttribute] void를 반환 하는 메서드를 사용할 수 없습니다.</td></tr>
        <tr><td>XA4209</td><td>클래스에 대 한 JavaTypeInfo를 만들지 못했습니다: {0} {1 \}</td></tr>
        <tr><td>XA4210</td><td>ExportAttribute 또는 ExportFieldAttribute 사용 하 여 Mono.Android.Export.dll에 대 한 참조를 추가 해야 합니다.</td></tr>
        <tr><td>XA4211</td><td>AndroidManifest.xml //uses-sdk/@android:targetSdkVersion ' {은 (는) ' $(TargetFrameworkVersion) '{1 \}' 보다 작습니다. API-사용 하 여 \ {1 \\} ACW 컴파일에 대 한 합니다.</td></tr>
    </tbody>
</table>

### <a name="xa5xxx-errors"></a>XA5xxx 오류

<table>
    <thead>
        <tr><td>오류 코드</td><td>설명</td></tr>
    </thead>
    <tbody>
        <tr><td>XA5101</td><td>누락 된 ' {'이 (0) 컴파일러입니다. Android NDK를 설치 하십시오.</td></tr>
        <tr><td>XA5102</td><td>어셈블리를 네이티브 코드로 변환 하지 못했습니다. 에 버그 보고서를 제출 하세요 <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5103</td><td>파일 ' {'이 (가) 컴파일하지 못했습니다. 에 버그 보고서를 제출 하세요 <a href="http://bugzilla.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5201</td><td>네이티브 연결에 실패 했습니다. Gcc에 제공 된 사용자 플래그를 검토 하십시오: {0}</td></tr>
        <tr><td>XA5202</td><td>네이티브 연결에 실패 했습니다. 빌드 로그를 검토 하십시오.</td></tr>
        <tr><td>XA5303</td><td>네이티브 연결 경고: {0}</td></tr>
        <tr><td>XA5300</td><td>Android SDK 찾을 수 없거나 설치 하지 않습니다.</td></tr>
        <tr><td>XA5301</td><td>누락 된 '제거' 도구입니다. Xcode '명령줄 도구' 구성 요소를 설치 하십시오</td></tr>
        <tr><td>XA5302</td><td>누락 된 'dsymutil' 도구입니다. Xcode '명령줄 도구' 구성 요소를 설치 하십시오</td></tr>
        <tr><td>XA5203</td><td>디버그 기호 (dSYM 디렉터리)를 생성 하지 못했습니다. 빌드 로그를 검토 하십시오.</td></tr>
        <tr><td>XA5204</td><td>최종 이진 파일을 제거 하지 못했습니다. 빌드 로그를 검토 하십시오.</td></tr>
        <tr><td>XA5205</td><td>누락 된 'aapt' 도구입니다. Android SDK 빌드-도구 패키지를 설치 하십시오.</td></tr>
        <tr><td>XA5206</td><td>{0}. Android 리소스 디렉터리 {1 \} 존재 하지 않습니다.</td></tr>
        <tr><td>XA5207</td><td>{0}. Java 라이브러리 파일 {1 \} 존재 하지 않습니다.</td></tr>
        <tr><td>XA5208</td><td>다운로드에 실패 했습니다. {0}를 다운로드 하 고 {1 \} 디렉터리에 배치 하세요.</td></tr>
        <tr><td>XA5209</td><td>압축을 풀기에 실패 했습니다. {0}를 다운로드 하 고 {1 \} 디렉터리에 추출 하세요.</td></tr>
        <tr><td>XA5210</td><td>{0}. 네이티브 라이브러리 파일 {1 \} 존재 하지 않습니다.</td></tr>
    </tbody>
</table>

### <a name="xa6xxx-errors"></a>XA6xxx 오류

<table>
    <thead>
        <tr><td>오류 코드</td><td>설명</td></tr>
    </thead>
    <tbody>
        <tr><td>XA6001</td><td>실행 중인 Cecil 버전이 제거 되는 어셈블리를 지원 하지 않습니다.</td></tr>
        <tr><td>XA6002</td><td>어셈블리를 제거 하지 못했습니다 `{0}`합니다.</td></tr>
        <tr><td>XA6003</td><td>UnauthorizedAccessException 메시지]</td></tr>
    </tbody>
</table>

### <a name="xa9xxx-errors"></a>XA9xxx 오류

이 오류 코드는 라이선싱 및 정품 인증 오류입니다.



#### <a name="xa9000"></a>XA9000

 **원인:** 라이선스가 만료

 **동안 검사:** 빌드

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>경고</td>
            <td>경고</td>
            <td>경고</td>
            <td>경고</td>
            <td>경고</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9001"></a>XA9001

 **원인:** 평가판이 만료

 **동안 검사:** 빌드

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>경고</td>
            <td>경고</td>
            <td>경고</td>
            <td>경고</td>
            <td>경고</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9002"></a>XA9002

 **원인:** AndroidJavaSource

 **동안 검사:** 빌드

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
        </tr>
    </tbody>
</table>

 **원인:** AndroidJavaLibrary

 **동안 검사:** 빌드

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
        </tr>
    </tbody>
</table>

 **바인딩 어셈블리에 포함 된.jar 경우 다음이 검색 되었습니다 빌드 시간이 아닌 패키지 시.**

 **원인:** AndroidNativeLibrary

 **동안 검사:** 빌드

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9003"></a>XA9003

 **원인:** System.Runtime.Serialization

 **동안 검사:** 패키지

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
        </tr>
    </tbody>
</table>

 **Cause:** System.ServiceModel.Web

 **동안 검사:** 패키지

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
        </tr>
    </tbody>
</table>

 **원인:** Mono.Data.Tds

 **동안 검사:** 빌드

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
        </tr>
    </tbody>
</table>

 **이 허용 System.Data.dll에서 참조**

 **원인:** Mono.Android.Export

 **동안 검사:** 빌드

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9004"></a>XA9004

 **원인:** -프로 파일링

 **동안 검사:** 패키지

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9005"></a>XA9005

 **원인:** 크기 제한 (32 kb)

 **동안 검사:** 패키지

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>확인</td>
            <td>-</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9006"></a>XA9006

 **원인:** System.Data.SqlClient 네임 스페이스

 **동안 검사:** 패키지

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9008"></a>XA9008

 **원인:** 명령줄에서 빌드

 **동안 검사:** 빌드

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>확인</td>
            <td>확인</td>
            <td>확인</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9009"></a>XA9009

 **원인:** Missing 일련 번호

 **동안 검사:** 볼

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9010"></a>XA9010

 **원인:** 잘못 된 제품 Id

 **동안 검사:** 빌드

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
        </tr>
    </tbody>
</table>

XA9018와 동일



#### <a name="xa9011"></a>XA9011

 **원인:** 라이선스 파일 (새 파일 형식)을 업데이트 하지 못했습니다.

 **동안 검사:** 정품 인증

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9012"></a>XA9012

 **원인:** 인터넷 없음

 **동안 검사:** 정품 인증

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9013"></a>XA9013

 **원인:** 알 수 없는 오류

 **동안 검사:** 정품 인증

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9014"></a>XA9014

 **원인:** 잘못 되었습니다. 활성화 코드

 **동안 검사:** 정품 인증

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
        </tr>
    </tbody>
</table>



#### <a name="xa9017"></a>XA9017

 **원인:** 정품 인증 서버에서 유효한 라이선스를 반환 하지 않습니다

 **동안 검사:** 정품 인증

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
            <td>오류</td>
        </tr>
    </tbody>
</table>


#### <a name="xa9018"></a>XA9018

**원인:** 잘못 된 라이선스

**동안 검사:** 빌드

<table>
    <thead>
        <tr>
            <th>시작</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>비즈니스</th>
            <th>엔터프라이즈</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-</td>
            <td>-</td>
            <td>오류</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>
