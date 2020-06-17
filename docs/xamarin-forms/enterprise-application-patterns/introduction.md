---
제목: "엔터프라이즈 앱 개발 소개" 설명: "이 장에서는 엔터프라이즈 앱 개발에 대 한 소개를 제공 하 고 eShopOnContainers 모바일 앱을 소개 합니다."
assetid: cbce0659-fa03-447a-86ec-140438143230 ms. 기술: xamarin-forms author: davidbritch: dabritch: ms. 날짜: 08/07/2017 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="introduction-to-enterprise-app-development"></a>엔터프라이즈 앱 개발 소개

플랫폼에 관계 없이 엔터프라이즈 앱 개발자는 다음과 같은 몇 가지 문제를 직면 하 고 있습니다.

- 시간이 지남에 따라 변경 될 수 있는 앱 요구 사항입니다.
- 새로운 비즈니스 기회 및 과제
- 앱의 범위와 요구 사항에 상당한 영향을 줄 수 있는 개발 중의 지속적인 피드백

이러한 점을 염두에 두면 시간이 지남에 따라 쉽게 수정 하거나 확장할 수 있는 앱을 빌드하는 것이 중요 합니다. 이러한 적응성을 설계 하는 것은 앱의 나머지 부분에 영향을 주지 않고 독립적으로 앱의 개별 부분을 독립적으로 개발 하 고 테스트할 수 있는 아키텍처가 필요 하기 때문에 어려울 수 있습니다.

많은 엔터프라이즈 앱은 둘 이상의 개발자를 요구 하기에 충분히 복잡 합니다. 앱을 디자인 하는 방법을 결정 하는 것은 여러 개발자가 앱의 여러 부분에서 독립적으로 작업할 수 있도록 하는 동시에 앱에 통합 된 경우에도 원활 하 게 연동 되도록 하는 것이 중요 합니다.

앱을 디자인 하 고 빌드하는 일반적인 접근 방식은 *모놀리식* 앱 이라고 하는 것으로, 구성 요소가 긴밀 하 게 분리 되지 않고 긴밀 하 게 결합 됩니다. 일반적으로이 모놀리식 접근 방식은 응용 프로그램의 다른 구성 요소를 중단 하지 않고 버그를 해결 하기 어려울 수 있으며, 새 기능을 추가 하거나 기존 기능을 대체 하기 어려울 수 있기 때문에 유지 관리가 어렵고 비효율적인 앱을 야기 합니다.

이러한 문제를 효과적으로 해결 하려면 앱을 쉽게 앱에 통합할 수 있는 느슨하게 결합 된 불연속 구성 요소로 분할 합니다. 이러한 접근 방식은 다음과 같은 여러 가지 이점을 제공 합니다.

- 개별 기능을 서로 다른 개인 이나 팀에서 개발, 테스트, 확장 및 유지 관리할 수 있습니다.
- 응용 프로그램의 가로 기능 (예: 인증 및 데이터 액세스), 앱 특정 비즈니스 기능 등의 세로 기능 간의 문제를 완전히 분리 하 고 재사용을 촉진 합니다. 이렇게 하면 응용 프로그램 구성 요소 간의 종속성 및 상호 작용을 보다 쉽게 관리할 수 있습니다.
- 다른 개인 이나 팀이 자신의 전문 지식에 따라 특정 작업 또는 기능에 초점을 맞출 수 있도록 하 여 역할의 분리를 유지 하는 데 도움이 됩니다. 특히 사용자 인터페이스와 앱의 비즈니스 논리를 명확 하 게 구분 합니다.

그러나 느슨하게 결합 된 불연속 구성 요소로 앱을 분할할 때 해결 해야 하는 많은 문제가 있습니다. 여기에는 다음이 포함됩니다.

- 사용자 인터페이스 컨트롤과 해당 논리 간의 문제를 명확 하 게 분리 하는 방법을 결정 합니다. 엔터프라이즈 앱을 만들 때 가장 중요 한 결정 사항 중 하나 Xamarin.Forms 는 코드 숨김으로 비즈니스 논리를 배치할지, 사용자 인터페이스 컨트롤과 논리 간의 문제를 명확 하 게 분리 하 여 앱을 유지 관리 하 고 테스트 가능 하 게 만드는 지 여부입니다. 자세한 내용은 [모델-뷰-ViewModel](~/xamarin-forms/enterprise-application-patterns/mvvm.md)을 참조 하세요.
- 종속성 주입 컨테이너를 사용할지 여부를 결정 합니다. 종속성 주입 컨테이너는 종속성이 주입 된 클래스의 인스턴스를 생성 하 고 컨테이너의 구성에 따라 수명 주기를 관리 하는 기능을 제공 하 여 개체 간의 종속성 결합을 줄입니다. 자세한 내용은 [종속성 주입](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)을 참조 하세요.
- 플랫폼에서 제공 하는 이벤트와 개체 및 형식 참조로 연결 하기 불편 한 구성 요소 간 느슨하게 결합 된 메시지 기반 통신을 선택 합니다. 자세한 내용은 [느슨하게 결합 한 구성 요소 간의 통신](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md)소개를 참조 하세요.
- 탐색 호출 방법 및 탐색 논리가 상주해 야 하는 위치를 포함 하 여 페이지 간에 탐색 하는 방법을 결정 합니다. 자세한 내용은 [탐색](~/xamarin-forms/enterprise-application-patterns/navigation.md)을 참조하세요.
- 사용자 입력의 유효성을 검사 하 여 정확성을 확인 하는 방법을 결정 합니다. 의사 결정은 사용자 입력의 유효성을 검사 하는 방법과 사용자에 게 유효성 검사 오류에 대해 알리는 방법을 포함 해야 합니다. 자세한 내용은 [유효성 검사](~/xamarin-forms/enterprise-application-patterns/validation.md)를 참조 하세요.
- 인증을 수행 하는 방법 및 권한 부여를 사용 하 여 리소스를 보호 하는 방법을 결정 합니다. 자세한 내용은 [인증 및 권한 부여](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md)를 참조 하세요.
- 데이터를 안정적으로 검색 하는 방법 및 데이터를 캐시 하는 방법을 비롯 하 여 웹 서비스에서 원격 데이터에 액세스 하는 방법을 결정 합니다. 자세한 내용은 [원격 데이터 액세스](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md)를 참조 하세요.
- 앱을 테스트 하는 방법을 결정 합니다. 자세한 내용은 [단위 테스트](~/xamarin-forms/enterprise-application-patterns/unit-testing.md)를 참조 하십시오.

이 가이드에서는 이러한 문제에 대 한 지침을 제공 하 고를 사용 하 여 플랫폼 간 엔터프라이즈 앱을 빌드하기 위한 핵심 패턴 및 아키텍처에 초점을 맞춘 것 Xamarin.Forms 입니다. 이 지침에서는 일반적인 Xamarin.Forms 엔터프라이즈 앱 개발 시나리오를 해결 하 고, MVVM (모델-뷰-ViewModel) 패턴 지원을 통해 프레젠테이션, 프레젠테이션 논리 및 엔터티의 문제를 구분 하 여 조정 가능 하 고, 유지 관리 가능 하 고, 테스트 가능한 코드를 생성 하는 데 도움이 됩니다.

## <a name="sample-application"></a>예제 응용 프로그램

이 가이드에는 다음 기능을 포함 하는 온라인 스토어 인 eShopOnContainers 샘플 응용 프로그램이 포함 되어 있습니다.

- 백 엔드 서비스에 대 한 인증 및 권한 부여
- Shirts, 커피 머그잔 및 기타 마케팅 항목의 카탈로그를 검색 합니다.
- 카탈로그를 필터링 합니다.
- 카탈로그에서 항목 순서 지정
- 사용자의 주문 기록 보기
- 설정 구성

### <a name="sample-application-architecture"></a>샘플 응용 프로그램 아키텍처

그림 1-1은 샘플 응용 프로그램의 아키텍처에 대 한 개략적인 개요를 제공 합니다.

![](introduction-images/architecture.png "eShopOnContainers high-level architecture")

**그림 1-1**: eShopOnContainers 상위 수준 아키텍처

샘플 응용 프로그램은 다음과 같은 세 개의 클라이언트 앱과 함께 제공 됩니다.

- ASP.NET Core를 사용 하 여 개발 된 MVC 응용 프로그램입니다.
- 2 차원 및 Typescript를 사용 하 여 개발 된 SPA (단일 페이지 응용 프로그램)입니다. 웹 응용 프로그램에 대 한이 방법은 각 작업으로 서버에 대 한 왕복을 수행 하지 않습니다.
- Xamarin.FormsIOS, Android 및 유니버설 Windows 플랫폼 (UWP)를 지 원하는를 사용 하 여 개발 된 모바일 앱입니다.

웹 응용 프로그램에 대 한 자세한 내용은 [ASP.NET Core 및 Microsoft Azure를 사용 하 여 최신 웹 응용 프로그램 설계 및 개발](https://aka.ms/WebAppEbook)을 참조 하세요.

샘플 응용 프로그램에는 다음과 같은 백 엔드 서비스가 포함 됩니다.

- ASP.NET Core Identity 및 IdentityServer를 사용 하는 id 마이크로 서비스입니다.
- 마이크로 서비스 카탈로그는 EntityFramework Core를 사용 하 여 SQL Server 데이터베이스를 사용 하는 데이터 기반 CRUD (만들기, 읽기, 업데이트, 삭제) 서비스입니다.
- 도메인 기반 디자인 패턴을 사용 하는 도메인 기반 서비스인 주문 마이크로 서비스.
- Redis Cache를 사용 하는 데이터 기반 CRUD 서비스인 바구니 마이크로 서비스.

이러한 백 엔드 서비스는 ASP.NET Core MVC를 사용 하 여 마이크로 서비스로 구현 되 고 단일 Docker 호스트 내에 고유한 컨테이너로 배포 됩니다. 이러한 백 엔드 서비스를 통칭 하 여 eShopOnContainers reference 응용 프로그램 이라고 합니다. 클라이언트 앱은 REST (Representational State Transfer) 웹 인터페이스를 통해 백 엔드 서비스와 통신 합니다. 마이크로 서비스 및 Docker에 대 한 자세한 내용은 [컨테이너 화 된 마이크로 서비스](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md)를 참조 하세요.

백 엔드 서비스의 구현에 대 한 자세한 내용은 [.Net 마이크로 서비스: 컨테이너 화 된 .Net 응용 프로그램 아키텍처](https://aka.ms/microservicesebook)를 참조 하세요.

### <a name="mobile-app"></a>모바일 앱

이 가이드에서는를 사용 하 여 플랫폼 간 엔터프라이즈 앱을 빌드하는 데 중점을 Xamarin.Forms eShopOnContainers 모바일 앱을 예로 사용 합니다. 그림 1-2에서는 앞에서 설명한 기능을 제공 하는 eShopOnContainers 모바일 앱의 페이지를 보여 줍니다.

[![](introduction-images/screenshots.png "The eShopOnContainers mobile app")](introduction-images/screenshots-large.png#lightbox "The eShopOnContainers mobile app")

**그림 1-2**: eShopOnContainers 모바일 앱

모바일 앱은 eShopOnContainers reference 응용 프로그램에서 제공 하는 백 엔드 서비스를 사용 합니다. 그러나 백 엔드 서비스 배포를 방지 하려는 사용자에 대해 모의 서비스의 데이터를 사용 하도록 구성할 수 있습니다.

EShopOnContainers 모바일 앱은 다음 기능을 Xamarin.Forms 수행 합니다.

- XAML
- 컨트롤
- 바인딩
- 컨버터
- 스타일
- 애니메이션
- 명령
- 동작
- 트리거
- 효과
- 사용자 지정 렌더러
- MessagingCenter
- 사용자 지정 컨트롤

이 기능에 대 한 자세한 내용은 [ Xamarin.Forms 설명서](~/xamarin-forms/index.yml)및를 [사용 하 Xamarin.Forms 여 Mobile Apps 만들기 ](https://aka.ms/xamformsebook)를 참조 하세요.

또한 eShopOnContainers 모바일 앱에서 일부 클래스에 대해 단위 테스트가 제공 됩니다.

#### <a name="mobile-app-solution"></a>모바일 앱 솔루션

EShopOnContainers 모바일 앱 솔루션은 소스 코드 및 기타 리소스를 프로젝트로 구성 합니다. 모든 프로젝트는 폴더를 사용 하 여 소스 코드 및 기타 리소스를 범주로 구성 합니다. 다음 표에서는 eShopOnContainers 모바일 앱을 구성 하는 프로젝트를 간략히 설명 합니다.

|Project|Description|
|--- |--- |
|eShopOnContainers|이 프로젝트는 공유 코드와 공유 UI를 포함 하는 PCL (이식 가능한 클래스 라이브러리) 프로젝트입니다.|
|eShopOnContainers|이 프로젝트는 android 관련 코드를 저장 하며 Android 앱의 진입점입니다.|
|eShopOnContainers|이 프로젝트는 ios 관련 코드를 보관 하며 iOS 앱에 대 한 진입점입니다.|
|eShopOnContainers|이 프로젝트는 UWP (유니버설 Windows 플랫폼) 관련 코드를 포함 하며 Windows 앱의 진입점입니다.|
|eShopOnContainers. Testrunner.completed|이 프로젝트는 eShopOnContainers 테스트 프로젝트에 대 한 Android test runner입니다.|
|eShopOnContainers. Testrunner.completed|이 프로젝트는 eShopOnContainers 테스트 프로젝트에 대 한 iOS test runner입니다.|
|eShopOnContainers. Testrunner.completed|이 프로젝트는 eShopOnContainers 테스트 프로젝트에 대 한 유니버설 Windows 플랫폼 test runner입니다.|
|eShopOnContainers 테스트|이 프로젝트에는 eShopOnContainers 프로젝트에 대 한 단위 테스트가 포함 되어 있습니다.|

EShopOnContainers 모바일 앱의 클래스는 전혀 수정 하지 않고 모든 앱에서 다시 사용할 수 있습니다 Xamarin.Forms .

##### <a name="eshoponcontainerscore-project"></a>eShopOnContainers 프로젝트

EShopOnContainers PCL 프로젝트에는 다음 폴더가 포함 되어 있습니다.

|폴더|Description|
|--- |--- |
|애니메이션|XAML에서 애니메이션을 사용 하도록 설정 하는 클래스를 포함 합니다.|
|동작|뷰 클래스에 노출 되는 동작을 포함 합니다.|
|컨트롤|앱에서 사용 하는 사용자 지정 컨트롤을 포함 합니다.|
|컨버터|바인딩에 사용자 지정 논리를 적용 하는 값 변환기를 포함 합니다.|
|효과|`EntryLineColorEffect`특정 컨트롤의 테두리 색을 변경 하는 데 사용 되는 클래스를 포함 `Entry` 합니다.|
|예외|사용자 지정를 포함 `ServiceAuthenticationException` 합니다.|
|확장|및 클래스에 대 한 확장 메서드를 포함 `VisualElement` `IEnumerable` 합니다.|
|도우미|앱에 대 한 도우미 클래스를 포함 합니다.|
|모델|앱에 대 한 모델 클래스를 포함 합니다.|
|속성|`AssemblyInfo.cs`.Net 어셈블리 메타 데이터 파일을 포함 합니다.|
|Services|응용 프로그램에 제공 되는 서비스를 구현 하는 인터페이스와 클래스를 포함 합니다.|
|트리거|`BeginAnimation`XAML에서 애니메이션을 호출 하는 데 사용 되는 트리거를 포함 합니다.|
|유효성 검사|데이터 입력의 유효성 검사와 관련 된 클래스를 포함 합니다.|
|ViewModels|페이지에 노출 되는 응용 프로그램 논리를 포함 합니다.|
|뷰|앱에 대 한 페이지를 포함 합니다.|

##### <a name="platform-projects"></a>플랫폼 프로젝트

플랫폼 프로젝트는 효과 구현, 사용자 지정 렌더러 구현 및 기타 플랫폼별 리소스를 포함 합니다.

## <a name="summary"></a>요약

Xamarin의 플랫폼 간 모바일 앱 개발 도구 및 플랫폼은 모든 대상 플랫폼 (iOS, Android 및 Windows)에서 코드를 공유 하는 기능을 제공 하 고 총 소유 비용을 낮출 수 있도록 하는 B2E, B2B 및 B2C mobile 클라이언트 앱에 대 한 종합적인 솔루션을 제공 합니다. 앱은 네이티브 플랫폼 모양과 느낌을 유지 하면서 사용자 인터페이스와 앱 논리 코드를 공유할 수 있습니다.

엔터프라이즈 앱 개발자는 개발 중에 앱의 아키텍처를 변경할 수 있는 몇 가지 문제를 직면 하 고 있습니다. 따라서 시간이 지남에 따라 앱을 수정 하거나 확장할 수 있도록 앱을 빌드하는 것이 중요 합니다. 이러한 적응성 설계는 어려울 수 있지만 일반적으로 앱에 쉽게 통합 될 수 있는 느슨하게 결합 된 개별 구성 요소로 앱을 분할 해야 합니다.

## <a name="related-links"></a>관련 링크

- [다운로드 전자책 (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
