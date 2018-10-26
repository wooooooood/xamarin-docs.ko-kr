---
title: Xamarin.Android 조각 연습-1 부
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: 984e72f2b3ee11bcc0902663893509e5687fc099
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119347"
---
# <a name="fragments-walkthrough-ndash-phone"></a>조각 연습 &ndash; 전화

세로 방향으로 Android 장치를 대상으로 하는 Xamarin.Android 앱을 만든 연습의 첫 번째 부분입니다. 이 연습에서는 Xamarin.Android에서 조각을 만드는 방법 및 샘플을 추가 하는 방법을 설명 합니다.

[![](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

이 앱에 대 한 다음과 같은 클래스가 만들어집니다.

1. `PlayQuoteFragment` &nbsp; 이 조각 William 셰익스피어에서 play에서 견적을 표시 됩니다. 호스트할 `PlayQuoteActivity`합니다.
1. `Shakespeare` &nbsp; 이 클래스는 두 개의 하드 코드 된 배열을 속성으로 유지 됩니다.
1. `TitlesFragment` &nbsp; 이 조각 William 셰익스피어 하 여 작성 된 재생의 타이틀의 목록을 표시 됩니다. 호스트할 `MainActivity`합니다.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` 시작 합니다 `PlayQuoteActivity` 에 대 한 응답에서 재생을 선택 하면 사용자에 게 `TitlesFragment`합니다.

## <a name="1-create-the-android-project"></a>1. Android 프로젝트 만들기

라는 새 Xamarin.Android 프로젝트를 만듭니다 **FragmentSample**합니다.
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![새 Xamarin.Android 프로젝트 만들기](./walkthrough-images/01-newproject.w157-sml.png)](./walkthrough-images/01-newproject.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![새 Xamarin.Android 프로젝트 만들기](./walkthrough-images/01-newproject.m742-sml.png)](./walkthrough-images/01-newproject.m742.png#lightbox)

선택 하는 것이 좋습니다 **최신 개발** 이 연습에 대 한 합니다.

프로젝트를 만든 후 파일 이름 바꾸기 **layout/Main.axml** 하 **layout/activity_main.axml**합니다.

-----

## <a name="2-add-the-data"></a>2. 데이터를 추가 합니다.

클래스 이름 속성 두 하드 코드 된 문자열 배열에서이 응용 프로그램에 대 한 데이터를 저장할 `Shakespeare`:

* `Shakespeare.Titles` &nbsp; 이 배열은 William 셰익스피어의 재생 목록이 포함 됩니다. 이 데이터 원본에는 `TitlesFragment`합니다.
* `Shakespeare.Dialogue` &nbsp; 이 배열에 포함 된 재생 중 하나에서 따옴표의 목록을 보유할 `Shakespeare.Titles`합니다. 이 데이터 원본에는 `PlayQuoteFragment`합니다.

새 C# 클래스를 **FragmentSample** 프로젝트 및 이름을 **Shakespeare.cs**합니다. 이 파일 내에서 만들 새 C# 라는 클래스 `Shakespeare` 같은 내용으로

```csharp
class Shakespeare
{
    public static string[] Titles = {
                                      "Henry IV (1)",
                                      "Henry V",
                                      "Henry VIII",
                                      "Richard II",
                                      "Richard III",
                                      "Merchant of Venice",
                                      "Othello",
                                      "King Lear"
                                    };

    public static string[] Dialogue = {
                                        "So shaken as we are, so wan with care, Find we a time for frighted peace to pant, And breathe short-winded accents of new broils To be commenced in strands afar remote. No more the thirsty entrance of this soil Shall daub her lips with her own children's blood; Nor more shall trenching war channel her fields, Nor bruise her flowerets with the armed hoofs Of hostile paces: those opposed eyes, Which, like the meteors of a troubled heaven, All of one nature, of one substance bred, Did lately meet in the intestine shock And furious close of civil butchery Shall now, in mutual well-beseeming ranks, March all one way and be no more opposed Against acquaintance, kindred and allies: The edge of war, like an ill-sheathed knife, No more shall cut his master. Therefore, friends, As far as to the sepulchre of Christ, Whose soldier now, under whose blessed cross We are impressed and engaged to fight, Forthwith a power of English shall we levy; Whose arms were moulded in their mothers' womb To chase these pagans in those holy fields Over whose acres walk'd those blessed feet Which fourteen hundred years ago were nail'd For our advantage on the bitter cross. But this our purpose now is twelve month old, And bootless 'tis to tell you we will go: Therefore we meet not now. Then let me hear Of you, my gentle cousin Westmoreland, What yesternight our council did decree In forwarding this dear expedience.",
                                        "Hear him but reason in divinity, And all-admiring with an inward wish You would desire the king were made a prelate: Hear him debate of commonwealth affairs, You would say it hath been all in all his study: List his discourse of war, and you shall hear A fearful battle render'd you in music: Turn him to any cause of policy, The Gordian knot of it he will unloose, Familiar as his garter: that, when he speaks, The air, a charter'd libertine, is still, And the mute wonder lurketh in men's ears, To steal his sweet and honey'd sentences; So that the art and practic part of life Must be the mistress to this theoric: Which is a wonder how his grace should glean it, Since his addiction was to courses vain, His companies unletter'd, rude and shallow, His hours fill'd up with riots, banquets, sports, And never noted in him any study, Any retirement, any sequestration From open haunts and popularity.",
                                        "I come no more to make you laugh: things now, That bear a weighty and a serious brow, Sad, high, and working, full of state and woe, Such noble scenes as draw the eye to flow, We now present. Those that can pity, here May, if they think it well, let fall a tear; The subject will deserve it. Such as give Their money out of hope they may believe, May here find truth too. Those that come to see Only a show or two, and so agree The play may pass, if they be still and willing, I'll undertake may see away their shilling Richly in two short hours. Only they That come to hear a merry bawdy play, A noise of targets, or to see a fellow In a long motley coat guarded with yellow, Will be deceived; for, gentle hearers, know, To rank our chosen truth with such a show As fool and fight is, beside forfeiting Our own brains, and the opinion that we bring, To make that only true we now intend, Will leave us never an understanding friend. Therefore, for goodness' sake, and as you are known The first and happiest hearers of the town, Be sad, as we would make ye: think ye see The very persons of our noble story As they were living; think you see them great, And follow'd with the general throng and sweat Of thousand friends; then in a moment, see How soon this mightiness meets misery: And, if you can be merry then, I'll say A man may weep upon his wedding-day.",
                                        "First, heaven be the record to my speech! In the devotion of a subject's love, Tendering the precious safety of my prince, And free from other misbegotten hate, Come I appellant to this princely presence. Now, Thomas Mowbray, do I turn to thee, And mark my greeting well; for what I speak My body shall make good upon this earth, Or my divine soul answer it in heaven. Thou art a traitor and a miscreant, Too good to be so and too bad to live, Since the more fair and crystal is the sky, The uglier seem the clouds that in it fly. Once more, the more to aggravate the note, With a foul traitor's name stuff I thy throat; And wish, so please my sovereign, ere I move, What my tongue speaks my right drawn sword may prove.",
                                        "Now is the winter of our discontent Made glorious summer by this sun of York; And all the clouds that lour'd upon our house In the deep bosom of the ocean buried. Now are our brows bound with victorious wreaths; Our bruised arms hung up for monuments; Our stern alarums changed to merry meetings, Our dreadful marches to delightful measures. Grim-visaged war hath smooth'd his wrinkled front; And now, instead of mounting barded steeds To fright the souls of fearful adversaries, He capers nimbly in a lady's chamber To the lascivious pleasing of a lute. But I, that am not shaped for sportive tricks, Nor made to court an amorous looking-glass; I, that am rudely stamp'd, and want love's majesty To strut before a wanton ambling nymph; I, that am curtail'd of this fair proportion, Cheated of feature by dissembling nature, Deformed, unfinish'd, sent before my time Into this breathing world, scarce half made up, And that so lamely and unfashionable That dogs bark at me as I halt by them; Why, I, in this weak piping time of peace, Have no delight to pass away the time, Unless to spy my shadow in the sun And descant on mine own deformity: And therefore, since I cannot prove a lover, To entertain these fair well-spoken days, I am determined to prove a villain And hate the idle pleasures of these days. Plots have I laid, inductions dangerous, By drunken prophecies, libels and dreams, To set my brother Clarence and the king In deadly hate the one against the other: And if King Edward be as true and just As I am subtle, false and treacherous, This day should Clarence closely be mew'd up, About a prophecy, which says that 'G' Of Edward's heirs the murderer shall be. Dive, thoughts, down to my soul: here Clarence comes.",
                                        "To bait fish withal: if it will feed nothing else, it will feed my revenge. He hath disgraced me, and hindered me half a million; laughed at my losses, mocked at my gains, scorned my nation, thwarted my bargains, cooled my friends, heated mine enemies; and what's his reason? I am a Jew. Hath not a Jew eyes? hath not a Jew hands, organs, dimensions, senses, affections, passions? fed with the same food, hurt with the same weapons, subject to the same diseases, healed by the same means, warmed and cooled by the same winter and summer, as a Christian is? If you prick us, do we not bleed? if you tickle us, do we not laugh? if you poison us, do we not die? and if you wrong us, shall we not revenge? If we are like you in the rest, we will resemble you in that. If a Jew wrong a Christian, what is his humility? Revenge. If a Christian wrong a Jew, what should his sufferance be by Christian example? Why, revenge. The villany you teach me, I will execute, and it shall go hard but I will better the instruction.",
                                        "Virtue! a fig! 'tis in ourselves that we are thus or thus. Our bodies are our gardens, to the which our wills are gardeners: so that if we will plant nettles, or sow lettuce, set hyssop and weed up thyme, supply it with one gender of herbs, or distract it with many, either to have it sterile with idleness, or manured with industry, why, the power and corrigible authority of this lies in our wills. If the balance of our lives had not one scale of reason to poise another of sensuality, the blood and baseness of our natures would conduct us to most preposterous conclusions: but we have reason to cool our raging motions, our carnal stings, our unbitted lusts, whereof I take this that you call love to be a sect or scion.",
                                        "Blow, winds, and crack your cheeks! rage! blow! You cataracts and hurricanoes, spout Till you have drench'd our steeples, drown'd the cocks! You sulphurous and thought-executing fires, Vaunt-couriers to oak-cleaving thunderbolts, Singe my white head! And thou, all-shaking thunder, Smite flat the thick rotundity o' the world! Crack nature's moulds, an germens spill at once, That make ingrateful man!"
                                    };
}
```

## <a name="3-create-the-playquotefragment"></a>3. PlayQuoteFragment 만들기

`PlayQuoteFragment` 응용 프로그램에서 이전에 사용자가 선택한 셰익스피어 재생에 대 한 따옴표를 표시 하는 Android 프래그먼트는이 조각이 Android 레이아웃 파일을 사용 하지는 않으면 대신 해당 사용자 인터페이스를 동적으로 생성 됩니다. 새 `Fragment` 라는 클래스 `PlayQuoteFragment` 프로젝트:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![새 C# 클래스](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/02-addclass.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![새 C# 클래스](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/02-addclass.m742.png#lightbox)

-----

그런 다음이 코드 조각은 유사 하 게 조각에 대 한 코드를 변경:

```csharp
public class PlayQuoteFragment : Fragment
{
    public int PlayId => Arguments.GetInt("current_play_id", 0);

    public static PlayQuoteFragment NewInstance(int playId)
    {
        var bundle = new Bundle();
        bundle.PutInt("current_play_id", playId);
        return new PlayQuoteFragment {Arguments = bundle};
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        if (container == null)
        {
            return null;
        }

        var textView = new TextView(Activity);
        var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
        textView.SetPadding(padding, padding, padding, padding);
        textView.TextSize = 24;
        textView.Text = Shakespeare.Dialogue[PlayId];

        var scroller = new ScrollView(Activity);
        scroller.AddView(textView);

        return scroller;
    }
}
```

이 조각 인스턴스화하기 위한 팩터리 메서드를 제공 하는 Android 앱에서 일반적인 패턴입니다. 이렇게 하면 조각을 적절 하 게 작동 하는 것에 대 한 필요한 매개 변수를 사용 하 여 만들어집니다. 이 연습에서는 앱에 사용 될는 `PlayQuoteFragment.NewInstance` 견적 선택 될 때마다 새 조각을 만드는 방법. 합니다 `NewInstance` 메서드는 단일 매개 변수는 &ndash; 표시할 견적의 인덱스입니다.

`OnCreateView` 화면의 조각을 렌더링할 때 Android에서 메서드가 호출 됩니다. Android 반환 `View` 개체 조각입니다. 이 조각에는 뷰를 만들려면 레이아웃 파일을 사용 하지 않습니다. 대신 프로그래밍 방식으로 만들어집니다 뷰 인스턴스화하여를 **TextView** 견적을 저장 하에서 해당 위젯을 표시 됩니다는 **ScrollView**합니다.

> [!NOTE]
> 조각 하위 클래스에는 매개 변수가 없는 공용 기본 생성자가 있어야 합니다.

## <a name="4-create-the-playquoteactivity"></a>4. PlayQuoteActivity 만들기

이 앱에 호스트 하는 작업에 필요 하므로 조각 활동 내에서 호스트 되어야 합니다는 `PlayQuoteFragment`합니다. 동적 활동 런타임에 레이아웃 조각을 추가 합니다. 응용 프로그램에 새 활동을 추가 하 고 이름을 `PlayQuoteActivity`:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![프로젝트에 Android 활동 추가](./walkthrough-images/03-addactivity.w157-sml.png)](./walkthrough-images/03-addactivity.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![프로젝트에 Android 활동 추가](./walkthrough-images/03-addactivity.m742-sml.png)](./walkthrough-images/03-addactivity.m742.png#lightbox)

-----

코드를 편집할 `PlayQuoteActivity`:

```csharp
[Activity(Label = "PlayQuoteActivity")]
public class PlayQuoteActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        var playId = Intent.Extras.GetInt("current_play_id", 0);

        var detailsFrag = PlayQuoteFragment.NewInstance(playId);
        FragmentManager.BeginTransaction()
                        .Add(Android.Resource.Id.Content, detailsFrag)
                        .Commit();
    }
}
```

때 `PlayQuoteActivity` 가 만들어지면 인스턴스화하고 새 `PlayQuoteFragment` 의 컨텍스트에서 해당 루트 뷰에서 해당 조각이 로드는 `FragmentTransaction`합니다. 이 활동의 사용자 인터페이스에 대 한 Android 레이아웃 파일을 로드 하지 않습니다 확인 합니다. 대신 새 `PlayQuoteFragment` 응용 프로그램의 루트 보기에 추가 됩니다. 리소스 식별자 `Android.Resource.Id.Content` 해당 특정 id를 알지 못해도 활동의 루트 뷰를 참조 하는 데 사용 됩니다.

## <a name="5-create-titlesfragment"></a>5. TitlesFragment 만들기

`TitlesFragment` 서브 클래스 라고 하는 특수화 된 조각 됩니다는 `ListFragment` 표시 하기 위한 논리를 캡슐화 하는 `ListView` 조각에 합니다. `ListFragment` 노출를 `ListAdapter` 속성 (에서 사용 하는 합니다 `ListView` 내용이 표시 되도록) 및 명명 된 이벤트 처리기 `OnListItemClick` 조각에 표시 되는 행에 대 한 클릭에 응답할 수 있는 `ListView`합니다.

시작 하려면 프로젝트에 새 조각을 추가 하 고 이름을 **TitlesFragment**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Android 추가 프로젝트에는 조각](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/04-addfragment.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Android 추가 프로젝트에는 조각](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/04-addfragment.m742.png#lightbox)

-----

조각 내의 코드를 편집 합니다.

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;

    public TitlesFragment()
    {
        // Being explicit about the requirement for a default constructor.
    }

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

Android는 호출 활동 만들어질 때를 `OnActivityCreated` 조각의; 메서드가 경우에 대 한 목록 어댑터를 `ListView` 만들어집니다.  `ShowQuoteFromPlay` 메서드는 인스턴스를 시작 합니다 `PlayQuoteActivity` 선택한 재생에 대 한 따옴표를 표시 하려면.

## <a name="display-titlesfragment-in-mainactivity"></a>MainActivity TitlesFragment 표시

마지막 단계는 표시할 `TitlesFragment` 내에서 `MainActivity`합니다. 활동은 조각을 동적으로 로드 되지 않습니다. 대신 조각 정적으로 로드 됩니다 사용 하 여 활동의 레이아웃 파일에서 선언 하 여는 `fragment` 요소입니다. 조각 로드를 설정 하 여 식별 되는 `android:name` 특성을 조각 클래스 (형식 네임 스페이스 포함). 예를 들어 사용 하는 `TitlesFragment`, 한 다음 `android:name` 로 설정 됩니다 `FragmentSample.TitlesFragment`합니다.

레이아웃 파일을 편집 **activity_mail.axml**, 다음을 사용 하 여 기존 XML을 대체 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <fragment
        android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

> [!NOTE]
> 합니다 `class` 특성은 대체 하는 유효한 `android:name`합니다. 정식 지침이 없는 형태를 기본 설정에 사용 하는 코드 베이스의 많은 예제 `class` 와 같은 의미로 `android:name`합니다.

MainActivity에 필요한 코드 변경 없이 있습니다. 해당 클래스의 코드는이 코드 조각과 매우 유사 해야 합니다.

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        SetContentView(Resource.Layout.activity_main);
    }
}
```

## <a name="run-the-app"></a>앱 실행

코드를 완료 했으므로 작동에서 하는지 확인 하려면 장치에서 앱을 실행 합니다.

[![휴대폰에서 실행 중인 응용 프로그램의 스크린샷입니다.](./walkthrough-images/05-app-screenshots-sml.png)](./walkthrough-images/05-app-screenshots.png#lightbox)

[이 연습의 2 부](./walkthrough-landscape.md) optimtize 가로 모드에서 실행 하는 장치에 대 한이 응용 프로그램을 됩니다.
