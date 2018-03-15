---
title: "Cognitive 서비스를 사용 하 여 인텔리전스 추가"
description: "Microsoft 인식 서비스는 Api, Sdk 및 안 면 인식, 음성 인식 및 언어 이해 같은 기능을 추가 하 여 응용 프로그램 지능적인을 개발자에 게 사용할 수 있는 서비스의 집합입니다. 이 문서는 일부의 Microsoft Cognitive 서비스 Api를 호출 하는 방법을 보여 주는 샘플 응용 프로그램에 대 한 소개를 제공 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: c309fb6936296dc181e499c91770ab8891121e9c
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="adding-intelligence-with-cognitive-services"></a>Cognitive 서비스를 사용 하 여 인텔리전스 추가

_Microsoft 인식 서비스는 Api, Sdk 및 안 면 인식, 음성 인식 및 언어 이해 같은 기능을 추가 하 여 응용 프로그램 지능적인을 개발자에 게 사용할 수 있는 서비스의 집합입니다. 이 문서는 일부의 Microsoft Cognitive 서비스 Api를 호출 하는 방법을 보여 주는 샘플 응용 프로그램에 대 한 소개를 제공 합니다._

## <a name="overview"></a>개요

함께 제공 된 샘플은 기능을 제공 하는 할 일 목록 응용 프로그램:

- 작업 목록을 봅니다.
- 추가 하 고 소프트 키보드를 통해 또는 Bing Speech api 음성 인식을 수행 하 여 작업을 편집 합니다. 음성 인식을 수행 하는 방법에 대 한 자세한 내용은 참조 [Bing Speech API를 사용 하 여 음성 인식](speech-recognition.md)합니다.
- Bing 맞춤법 검사 API를 사용 하 여 확인 작업 사용 하지 말고 이름입니다. 자세한 내용은 참조 [Bing 맞춤법 검사 API를 사용 하 여 맞춤법 검사](spell-check.md)합니다.
- 영어에서 변환기 API를 사용 하 여 독일어로 작업을 변환 합니다. 자세한 내용은 참조 [변환기 API를 사용 하 여 텍스트 번역](text-translation.md)합니다.
- 작업을 삭제 합니다.
- '완료'에 작업의 상태를 설정 합니다.
- Emotion 인식, Emotion API를 사용 하 여 응용 프로그램을 평가 합니다. 자세한 내용은 참조 [Emotion API를 사용 하 여 Emotion 인식](emotion-recognition.md)합니다.

작업은 로컬 SQLite 데이터베이스에 저장 됩니다. 로컬 SQLite 데이터베이스를 사용 하는 방법에 대 한 자세한 내용은 참조 [로컬 데이터베이스 작업을](~/xamarin-forms/app-fundamentals/databases.md)합니다.

`TodoListPage` 응용 프로그램을 시작할 때 표시 됩니다. 이 페이지는 로컬 데이터베이스에 저장 된 작업의 목록을 표시 하 고 사용자 응용 프로그램 등급을 지정 하거나 새 작업을 만들 수 있습니다.

![](images/sample-application-1.png "TodoListPage")

클릭 하 여 새 항목을 만들 수 있습니다는  *+*  를 탐색 하는 단추는 `TodoItemPage`합니다. 또한이 페이지는 작업을 선택 하 여를 탐색할 수도 있습니다.

![](images/sample-application-2.png "TodoItemPage")

`TodoItemPage` 번역, 저장 및 삭제 된 작업 만들기, 편집, 맞춤법 검사를 허용 합니다. 만들거나 작업을 편집할 음성 인식을 사용할 수 있습니다. 이렇게, 기록을 시작 하려면 마이크 단추를 눌러 및 키를 눌러 같은 단추를 두 번째로, 기록을 중지 하려면 Bing 음성 인식 API를 녹음/녹화 보내는 수신 하는 합니다.

Smilies 단추를 클릭 하 고 `TodoListPage` 로 이동는 `RateAppPage`, emotion 인식 안 면 식의 이미지에서 수행 하는 동안 사용 되는:

![](images/sample-application-3.png "RateAppPage")

`RateAppPage` 표시 되 고 반환 된 emotion 사용 하 여 Emotion API에 전송 되는 해당 얼굴 사진 걸릴 수 있습니다.

## <a name="understanding-the-application-anatomy"></a>응용 프로그램 분석 이해

샘플 응용 프로그램에 대 한 PCL 이식 가능한 클래스 라이브러리 () 프로젝트 5 개의 주 폴더가 이루어져 있습니다.

|폴더|용도|
|--- |--- |
|모델|응용 프로그램에 대 한 데이터 모델 클래스를 포함합니다. 여기에 `TodoItem` 응용 프로그램에서 사용 되는 데이터의 단일 항목을 모델링 하는 클래스입니다. 폴더에는 모델 JSON 응답의 다른 Microsoft Cognitive 서비스 Api에서 반환 하는 데 사용 되는 클래스도 포함 됩니다.|
|저장소|포함 된 `ITodoItemRepository` 인터페이스 및 `TodoItemRepository` 데이터베이스 작업을 수행 하는 데 사용 되는 클래스입니다.|
|서비스|다른 Microsoft Cognitive 서비스 Api에서 사용 되는 인터페이스와 함께 액세스 하는 데 사용 되는 클래스 및 인터페이스가 들어는 `DependencyService` 클래스 플랫폼 프로젝트에서 인터페이스를 구현 하는 클래스를 찾으려고 합니다.|
|유틸리티|포함 된 `Timer` 클래스에서 사용 되는 `AuthenticationService` 9 분 마다 JWT 액세스 토큰을 갱신 하는 클래스입니다.|
|보기|응용 프로그램 페이지가 포함 됩니다.|

PCL 프로젝트에는 몇 가지 중요 한 파일이 들어 있습니다.

|파일|용도|
|--- |--- |
|Constants.cs|`Constants` Microsoft Cognitive 서비스 api 호출 되는 API 키와 끝점을 지정 하는 클래스입니다. API 키 상수 다른 Cognitive 서비스 Api에 액세스 하려면 업데이트 해야 합니다.|
|App.xaml.cs|`App` 클래스는 각 플랫폼에서 응용 프로그램에 의해 표시 되는 두는 첫 번째 페이지를 인스턴스화 및 `TodoManager` 데이터베이스 작업을 호출 하는 데 사용 되는 클래스입니다.|

### <a name="nuget-packages"></a>NuGet 패키지

샘플 응용 프로그램에서는 다음과 같은 NuGet 패키지를 사용합니다.

- `Microsoft.Net.Http` – 제공 된 `HttpClient` HTTP를 통해 요청을 만들기 위한 클래스입니다.
- `Newtonsoft.Json` –.NET에 대 한 JSON 프레임 워크를 제공 합니다.
- `Microsoft.ProjectOxford.Emotion` – Emotion API에 액세스 하기 위한 클라이언트 라이브러리입니다.
- `PCLStorage` – 플랫폼 간 로컬 파일 IO Api 집합을 제공 합니다.
- `sqlite-net-pcl` – SQLite 데이터베이스 저장소를 제공 합니다.
- `Xam.Plugin.Media` – 제공 플랫폼 간 사진 기록 및 Api를 선택 합니다.

또한 이러한 NuGet 패키지는 또한 자신의 종속성을 설치합니다.

### <a name="modeling-the-data"></a>데이터 모델링

예제 응용 프로그램 사용은 `TodoItem` 표시 되며 로컬 SQLite 데이터베이스에 저장 된 데이터를 모델링 하는 클래스입니다. 다음 코드 예제는 `TodoItem` 클래스를 보여줍니다.

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

`ID` 속성은 각 고유 하 게 식별 하는 데 사용 `TodoItem` 인스턴스를 선택한 데이터베이스의 자동 증분 기본 키 속성을 구성 하는 SQLite 특성으로 데코레이팅되 어 있습니다.

### <a name="invoking-database-operations"></a>데이터베이스 작업을 호출

`TodoItemRepository` 클래스는 데이터베이스 작업을 구현 하 고 클래스의 인스턴스를 통해 액세스할 수는 `App.TodoManager` 속성입니다. `TodoItemRepository` 클래스는 데이터베이스 작업을 호출할 메서드를 제공 합니다.

- **GetAllItemsAsync** – 로컬 SQLite 데이터베이스에서 모든 항목을 검색 합니다.
- **GetItemAsync** – 로컬 SQLite 데이터베이스에서 지정된 된 항목을 검색 합니다.
- **SaveItemAsync** – 만들거나 로컬 SQLite 데이터베이스에 있는 항목을 업데이트 합니다.
- **DeleteItemAsync** – 로컬 SQLite 데이터베이스에서 지정 된 항목을 삭제 합니다.

### <a name="platform-project-implementations"></a>플랫폼 프로젝트 구현

`Services` 폴더 PCL 프로젝트에 포함는 `IFileHelper` 및 `IAudioRecorderService` 에서 사용 되는 인터페이스는 `DependencyService` 클래스 플랫폼 프로젝트에서 인터페이스를 구현 하는 클래스를 찾으려고 합니다.

`IFileHelper` 인터페이스에서 구현 됩니다는 `FileHelper` 각 플랫폼 프로젝트의 클래스. 이 클래스는 단일 메서드로 구성 `GetLocalFilePath`, SQLite 데이터베이스에 저장 하기 위한 로컬 파일 경로 반환 하는 합니다.

`IAudioRecorderService` 인터페이스에서 구현 됩니다는 `AudioRecorderService` 각 플랫폼 프로젝트의 클래스. 이 클래스 이루어져 `StartRecording`, `StopRecording`, 및 장치의 마이크의 오디오를 기록할 플랫폼 Api를 사용 하 고 wav 파일으로 저장 하는 메서드를 지원 합니다. IOS의 경우에 `AudioRecorderService` 사용 하 여는 `AVFoundation` 오디오를 기록할 API. Android에서는 `AudioRecordService` 사용 하 여는 `AudioRecord` 오디오 녹음 하는 API입니다. 에 플랫폼 UWP (유니버설 Windows)는 `AudioRecorderService` 사용 하 여는 `AudioGraph` 오디오 녹음 하는 API입니다.

### <a name="invoking-cognitive-services"></a>Cognitive 서비스 호출

샘플 응용 프로그램은 다음 Microsoft Cognitive 서비스를 호출합니다.

- Bing Speech API입니다. 자세한 내용은 참조 [Bing Speech API를 사용 하 여 음성 인식](speech-recognition.md)합니다.
- Bing 맞춤법 검사 API입니다. 자세한 내용은 참조 [Bing 맞춤법 검사 API를 사용 하 여 맞춤법 검사](spell-check.md)합니다.
- API를 변환 합니다. 자세한 내용은 참조 [변환기 API를 사용 하 여 텍스트 번역](text-translation.md)합니다.
- Emotion API입니다. 자세한 내용은 참조 [Emotion API를 사용 하 여 Emotion 인식](emotion-recognition.md)합니다.


## <a name="related-links"></a>관련 링크

- [Microsoft Cognitive 서비스 설명서](https://www.microsoft.com/cognitive-services/documentation)
- [Todo Cognitive 서비스 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
