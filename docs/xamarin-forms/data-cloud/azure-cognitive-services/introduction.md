---
title: Xamarin.ios 및 Azure Cognitive Services 소개
description: 이 문서에서는 일부 Microsoft Cognitive 서비스 Api를 호출 하는 방법을 보여 주는 샘플 응용 프로그램을 소개 합니다.
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 52774b387644b14e3d4612dffa6d3c3b28a37f25
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "68652306"
---
# <a name="xamarinforms-and-azure-cognitive-services-introduction"></a>Xamarin.ios 및 Azure Cognitive Services 소개

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Cognitive Services는 Api, Sdk 및 얼굴 인식, 음성 인식 및 언어 이해와 같은 기능을 추가 하 여 더 지능적인 응용 프로그램을 확인 하는 개발자에 게 사용할 수 있는 서비스의 집합입니다. 이 문서에서는 일부 Microsoft Cognitive 서비스 Api를 호출 하는 방법을 보여 주는 샘플 응용 프로그램에 대 한 소개를 제공 합니다._

## <a name="overview"></a>개요

함께 제공 되는 샘플은 기능을 제공 하는 할 일 목록 응용 프로그램:

- 작업의 목록을 봅니다.
- 추가 하 고 소프트 키보드를 통해 또는 Microsoft Speech API를 사용 하 여 음성 인식을 수행 하 여 작업을 편집 합니다. 음성 인식을 수행 하는 방법에 대 한 자세한 내용은 참조 하세요. [Microsoft Speech API를 사용 하 여 음성 인식](speech-recognition.md)합니다.
- Bing Spell Check API를 사용 하 여 확인 작업을 맞춤법 합니다. 자세한 내용은 [Bing Spell Check API를 사용 하 여 맞춤법 검사](spell-check.md)합니다.
- Translator API를 사용 하 여 독일어로 영어에서 작업을 변환 합니다. 자세한 내용은 [텍스트 번역, Translator API를 사용 하 여](text-translation.md)입니다.
- 작업을 삭제 합니다.
- '실행' 작업의 상태를 설정 합니다.
- Face API를 사용 하 여 감정 인식 기능을 사용 하 여 응용 프로그램을 평가 합니다. 자세한 내용은 [Face API를 사용 하 여 감정 인식](emotion-recognition.md)합니다.

작업은 로컬 SQLite 데이터베이스에 저장 됩니다. 로컬 SQLite 데이터베이스를 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [로컬 데이터베이스를 사용 하 여 작업](~/xamarin-forms/data-cloud/data/databases.md)합니다.

`TodoListPage` 응용 프로그램을 시작할 때 표시 됩니다. 이 페이지는 로컬 데이터베이스에 저장 된 작업의 목록을 표시 하 고를 사용 하면 새 작업 만들기 또는 응용 프로그램 평가:

![](introduction-images/sample-application-1.png "TodoListPage")

새 항목을 클릭 하 여 만들 수 있습니다는 *+* 이동 하는 단추는 `TodoItemPage`합니다. 또한이 페이지는 작업을 선택 하 여를 탐색할 수 있습니다.

![](introduction-images/sample-application-2.png "TodoItemPage")

`TodoItemPage` 변환, 저장 및 삭제 작업 만들기, 편집, 맞춤법 검사를 허용 합니다. 음성 인식 만들기 또는 편집 작업을 사용할 수 있습니다. 이렇게을 기록 하려면 마이크 단추를 눌러와 같은 단추를 다시 누르면 녹음을 중지 하 여 Bing 음성 인식 API를 녹음/녹화를 보냅니다는 합니다.

Smilies 단추를 클릭 하는 `TodoListPage` 으로 이동 합니다 `RateAppPage`, 얼굴 식의 이미지에서 감정 인식 하는 데 사용 되는:

![](introduction-images/sample-application-3.png "RateAppPage")

`RateAppPage` Face API에 표시 되 고 반환 된 emotion 함께 전송 되는 해당 얼굴 사진을 촬영할 수 있습니다.

## <a name="understand-the-application-anatomy"></a>응용 프로그램 분석 이해

샘플 응용 프로그램에 대 한 공유 코드 프로젝트는 다음과 같은 5 개의 기본 폴더로 구성 됩니다.

|폴더|용도|
|--- |--- |
|모델|응용 프로그램에 대 한 데이터 모델 클래스를 포함합니다. 여기에 `TodoItem` 응용 프로그램에서 사용 되는 데이터의 단일 항목을 모델링 하는 클래스입니다. 폴더에는 다른 Microsoft Cognitive 서비스 Api에서 반환 된 모델 JSON 응답 하는 데 사용 되는 클래스도 포함 됩니다.|
|리포지토리|포함 된 `ITodoItemRepository` 인터페이스 및 `TodoItemRepository` 데이터베이스 작업을 수행 하는 데 사용 되는 클래스입니다.|
|서비스|인터페이스 및 다른 Microsoft Cognitive Service Api를 함께 사용 되는 인터페이스에 액세스 하는 데 사용 되는 클래스를 포함 합니다 `DependencyService` 플랫폼 프로젝트에서 인터페이스를 구현 하는 클래스를 찾을 클래스입니다.|
|유틸리티|포함 된 `Timer` 클래스에서 사용 되는 `AuthenticationService` 9 분 마다 JWT 액세스 토큰을 갱신 하는 클래스입니다.|
|보기|응용 프로그램 페이지가 포함 됩니다.|

공유 코드 프로젝트에는 몇 가지 중요 한 파일도 포함 되어 있습니다.

|파일|용도|
|--- |--- |
|Constants.cs|`Constants` 호출 되는 Microsoft Cognitive Service Api에 대 한 API 키 및 끝점을 지정 하는 클래스입니다. API 키 상수는 다른 Cognitive 서비스 Api에 액세스를 업데이트 해야 합니다.|
|App.xaml.cs|합니다 `App` 클래스는 각 플랫폼에서 응용 프로그램에서 표시 되는 두는 첫 번째 페이지를 인스턴스화를 담당 하며 `TodoManager` 데이터베이스 작업을 호출 하는 데 사용 되는 클래스입니다.|

### <a name="nuget-packages"></a>NuGet 패키지

샘플 응용 프로그램에는 다음 NuGet 패키지를 사용합니다.

- `Newtonsoft.Json` –.NET에 대 한 JSON 프레임 워크를 제공 합니다.
- `PCLStorage` -플랫폼 간 로컬 파일 IO Api 집합을 제공 합니다.
- `sqlite-net-pcl` -SQLite 데이터베이스 저장소를 제공 합니다.
- `Xam.Plugin.Media` -플랫폼 간 사진 수행 및 Api를 선택 합니다. 제공 합니다.

또한 이러한 NuGet 패키지는 또한 자체 종속성을 설치 합니다.

### <a name="model-the-data"></a>데이터 모델링

샘플 응용 프로그램을 `TodoItem` 표시 되 고 로컬 SQLite 데이터베이스에 저장 하는 데이터를 모델링 하는 클래스입니다. 다음 코드 예제는 `TodoItem` 클래스를 보여줍니다.

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

합니다 `ID` 속성 각각을 고유 하 게 식별 하는 데 사용 됩니다 `TodoItem` 인스턴스 및 데이터베이스의 자동 증분 기본 키 속성을 확인 하는 SQLite 특성으로 데코레이팅됩니다.

### <a name="invoke-database-operations"></a>데이터베이스 작업 호출

합니다 `TodoItemRepository` 데이터베이스 작업을 구현 하는 클래스 및 클래스의 인스턴스를 통해 액세스할 수는 `App.TodoManager` 속성입니다. `TodoItemRepository` 클래스는 데이터베이스 작업을 호출할 메서드를 제공 합니다.

- **GetAllItemsAsync** – 로컬 SQLite 데이터베이스에서 모든 항목을 검색 합니다.
- **GetItemAsync** – 로컬 SQLite 데이터베이스에서 지정된 된 항목을 검색 합니다.
- **SaveItemAsync** – 만들거나 로컬 SQLite 데이터베이스에서 항목을 업데이트 합니다.
- **DeleteItemAsync** – 로컬 SQLite 데이터베이스에서 지정 된 항목을 삭제 합니다.

### <a name="platform-project-implementations"></a>플랫폼 프로젝트 구현

공유 코드 프로젝트의 `IFileHelper` `IAudioRecorderService` `DependencyService` 폴더에는 클래스에서 플랫폼 프로젝트의 인터페이스를 구현 하는 클래스를 찾기 위해 사용 하는 및 인터페이스가 포함 되어 있습니다. `Services`

합니다 `IFileHelper` 가 인터페이스를 구현 합니다 `FileHelper` 각 플랫폼 프로젝트에서 클래스입니다. 이 클래스는 단일 메서드로 구성 `GetLocalFilePath`, SQLite 데이터베이스를 저장 하는 것에 대 한 로컬 파일 경로 반환 합니다.

합니다 `IAudioRecorderService` 가 인터페이스를 구현 합니다 `AudioRecorderService` 각 플랫폼 프로젝트에서 클래스입니다. 이 클래스 `StartRecording`, `StopRecording`, 및 장치의 마이크에서 오디오를 기록할 플랫폼 Api를 사용 하 고 wav 파일로 저장 하는 메서드를 지원 합니다. Ios의 경우는 `AudioRecorderService` 사용 하는 `AVFoundation` 오디오 녹음 API. Android에는 `AudioRecordService` 사용 하 여는 `AudioRecord` 오디오 녹음 API. Windows 플랫폼 (UWP (유니버설)을 합니다 `AudioRecorderService` 사용을 `AudioGraph` 오디오 녹음 API.

### <a name="invoke-cognitive-services"></a>인식 서비스 호출

다음 Microsoft Cognitive Services를 호출 하는 샘플 응용 프로그램:

- Microsoft Speech API입니다. 자세한 내용은 [Microsoft Speech API를 사용 하 여 음성 인식](speech-recognition.md)합니다.
- Bing Spell Check API 자세한 내용은 [Bing Spell Check API를 사용 하 여 맞춤법 검사](spell-check.md)합니다.
- API를 변환 합니다. 자세한 내용은 [텍스트 번역, Translator API를 사용 하 여](text-translation.md)입니다.
- Face API입니다. 자세한 내용은 [Face API를 사용 하 여 감정 인식](emotion-recognition.md)합니다.

## <a name="related-links"></a>관련 링크

- [Microsoft Cognitive Services 설명서](https://www.microsoft.com/cognitive-services/documentation)
- [Todo Cognitive Services (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
