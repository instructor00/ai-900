# Language Studio에서 질문 답변 모델 사용

이 연습에서는 Language Studio를 사용하여 고객 서비스 봇에서 사용할 질문과 답변에 대한 기술 자료를 만들고 학습합니다. 기술 자료의 콘텐츠는 가상 여행사인 Margie's Travel 웹 사이트의 기존 FAQ 페이지에서 제공됩니다. 그런 다음 Language Studio를 사용하여 고객이 사용할 때 어떻게 작동하는지 확인합니다.

봇을 구현할 때 첫 번째 단계는 질문과 답변 쌍으로 구성된 기술 자료를 만드는 것입니다. 이는 기본 제공된 자연어 처리 기능과 함께 사용되어 봇이 질문을 해석하고 사용자에게 가장 적절한 답변을 찾을 수 있습니다.

Azure AI 언어에는 기술 자료를 만드는 데 사용할 수 있는 *질문 답변* 기능이 포함되어 있습니다. 기술 자료는 질문과 답변 쌍을 수동으로 입력하거나 기존 문서나 웹 페이지를 통해 만들 수 있습니다. Margie's Travel은 기존 FAQ 문서를 사용하려고 합니다.

언어 서비스의 질문 답변 기능을 사용하면 질문 및 답변 쌍을 입력하거나 기존 문서나 웹 페이지를 입력하여 기술 자료를 빠르게 만들 수 있습니다. 그런 다음, 몇 가지 기본 제공 자연어 처리 기능을 사용하여 질문을 해석하고 적절한 답을 찾을 수 있습니다.

## *Language* 리소스 만들기

질문 답변을 사용하려면 **언어** 리소스가 필요합니다.

1. 다른 브라우저 탭에서 [https://portal.azure.com](https://portal.azure.com?azure-portal=true)의 Azure Portal을 열고 Azure 구독과 연결된 Microsoft 계정으로 로그인합니다.

1. **&#65291;리소스 만들기** 단추를 클릭하고 *언어 서비스*를 검색합니다. **언어 서비스** 계획 **만들기**를 선택합니다. **추가 기능을 선택**할 수 있는 페이지로 이동됩니다. 다음 설정을 사용합니다.
    - **추가 기능 선택**:
        - **기본 기능**: 기본 기능 유지**
        - **사용자 지정 기능**: 사용자 지정 질문 답변을 선택합니다**.
     - **계속하여 리소스 만들기**
    ![사용자 지정 질문 답변이 사용하도록 설정된 언어 서비스 리소스 만들기](media/create-a-bot/create-language-service-resource.png)를 선택합니다.

1. **언어 만들기** 페이지에서 다음 설정을 지정합니다.
    - **프로젝트 세부 정보**
        - **구독**: *자신의 Azure 구독*.
        - **리소스 그룹**: 기존 *리소스 그룹*을 선택하거나 새 리소스 그룹을 만듭니다.
    - **인스턴스 세부 정보**
        - **지역**: *지역을 선택합니다. 미국 동부에 있는 경우 "미국 동부 2"를 사용합니다.*      
        - **이름**: 언어 서비스의 고유한 이름**
        - **가격 책정 계층**: S(분당 호출 1,000개)
    - **사용자 지정 질문 답변**
        - **Azure Search 지역**: 사용 가능한 모든 위치**
        - **Azure Search 가격 책정 계층**: 무료 F(인덱스 3개) -(*이 계층을 사용할 수 없는 경우 기본 선택*)
    - **책임 있는 AI 알림**
        - **이 확인란을 선택하여 책임 있는 AI 알림의 조항을 검토하고 승인했음을 확인**: **: 선택하였습니다.

1. **검토 및 만들기**를 선택한 후 **만들기**를 선택합니다. 사용자 지정 질문 답변 기술 자료를 지원할 언어 서비스가 배포될 때까지 기다립니다.

    > **참고** 무료 계층 **Azure Cognitive Search** 리소스를 이미 프로비전한 경우 할당량으로 인해 다른 리소스를 만들 수 없을 수 있습니다. 이 경우 **무료 F** 이외의 계층을 선택합니다.

## 새 프로젝트 만들기

1. 새 브라우저 탭에서 [https://language.azure.com](https://language.azure.com?azure-portal=true)의 Language Studio 포털을 열고 Azure 구독과 연결된 Microsoft 계정을 사용하여 로그인합니다.
1. 언어 리소스를 선택하라는 메시지가 표시되면 다음 설정을 선택합니다.
    - **Azure 디렉터리**: *구독이 포함된 Azure 디렉터리입니다*.
    - **Azure 구독**: *사용자의 Azure 구독*.
    - **언어 리소스**: *이전에 만든 언어 리소스.*

    언어 리소스를 선택하라는 메시지가 표시되지 ***않으면*** 구독에 여러 언어 리소스가 있기 때문일 수 있습니다. 이 경우 다음을 수행합니다.
    1. 페이지 상단 표시줄에서 **설정(&#9881;)** 을 선택합니다.      
    1. **설정** 페이지에서 **리소스** 탭을 봅니다.
    1. 방금 만든 언어 리소스를 선택하고 **리소스 전환**을 선택합니다.
    1. 페이지 상단에서 **Language Studio**를 선택하여 Language Studio 홈페이지로 돌아갑니다.

1. Language Studio 포털의 위쪽에 있는 **새로 만들기** 메뉴에서 **사용자 지정 질문 답변**을 선택합니다.

    ![사용자 지정 질문 답변](media/create-a-bot/create-custom-question-answering.png)

1. **리소스의 언어 설정 선택** 페이지에서 **이 리소스에서 프로젝트를 만들 때 언어를 선택하겠습니다**를 선택하고 **다음**을 클릭합니다.**
  ![언어를 선택하려고 합니다.](media/create-a-bot/create-project.png)

1. **기본 정보 입력** 페이지에서 다음 세부 정보를 입력하고 **다음**을 클릭합니다.
    - **언어 리소스**: 언어 리소스를 선택합니다.**  
    - **Azure Search 리소스**: Azure Search 리소스를 선택합니다.**
    - **이름**: `MargiesTravel`
    - **설명**: `A simple knowledge base`
    - **원본 언어**: 영어
    - **답변이 반환되지 않는 경우의 기본 답변**: `No answer found`
1. **검토 및 완료** 페이지에서 **프로젝트 만들기**를 선택합니다.
1. **원본 관리** 페이지가 표시됩니다. **&#65291;원본 추가**를 선택하고 **URL**을 선택합니다.
1. **URL 추가** 상자에서 **+ URL 추가**를 선택합니다. 다음을 입력하고 **모두 추가**를 선택합니다.
    - **URL 이름**: `MargiesKB`
    - **URL**: `https://raw.githubusercontent.com/MicrosoftLearning/mslearn-ai-fundamentals/main/data/natural-language/margies_faq.docx`
    - **파일 구조 분류**: 자동 검색**
1. **모두 추가**를 선택합니다.  

 ![URL 추가](media/create-a-bot/add-url.png)

## 기술 자료 테스트

기술 자료는 FAQ 문서의 세부 정보 및 미리 정의된 몇 가지 응답을 기반으로 합니다. 사용자 지정 질문 및 답변 쌍을 추가하여 이를 보완할 수 있습니다.

1. 왼쪽 패널을 확장하고 **기술 자료 편집**을 선택합니다. 그런 다음 **+** 을 선택하여 새 질문 쌍을 추가합니다.
1. **새 질문 답변 쌍 추가** 대화 상자에서 **질문**에 `Hello`를 입력하고 **답변**에 `Hi`를 입력한 다음 **완료**를 선택합니다.
1. **대체 질문**을 확장하고 **+ 대체 질문 추가**를 선택합니다. 그런 다음 "Hello"에 대한 대체 문구로 `Hiya`를 입력합니다.
1. **질문 답변 쌍** 창 상단에서 **저장**을 선택하여 기술 자료를 저장합니다.

## 기술 자료 학습 및 테스트

이제 기술 자료가 있으므로 테스트할 수 있습니다.

1. **질문 답변 쌍** 창 상단에서 **테스트**를 선택하여 기술 자료를 테스트합니다.
1. 테스트 창의 맨 아래에 `Hi` 메시지를 입력합니다. *안녕하세요* 응답이 반환되어야 합니다.
1. 테스트 창의 맨 아래에 `I want to book a flight` 메시지를 입력합니다. FAQ에서 적절한 응답이 반환되어야 합니다.

    > **참고** 응답에는 짧은 답변과 보다 자세한 답변 구절이 포함됩니다. 답변 구절은 가장 근접하게 일치하는 질문에 대한 FAQ 문서의 전체 텍스트를 표시하고, 짧은 답변은 해당 구절에서 지능적으로 추출됩니다.**** 테스트 창 맨 위에 있는 **간단한 대답 표시** 확인란을 사용하여 응답의 짧은 답변을 제어할 수 있습니다.

1. `How can I cancel a reservation?`과 같은 다른 질문을 시도해 보세요.
1. 기술 자료 테스트가 완료되면 **테스트**를 선택하여 테스트 창을 닫습니다.

## 기술 자료용 봇 만들기

기술 자료에서는 클라이언트 애플리케이션이 사용자 인터페이스를 통해 질문에 대답하는 데 사용할 수 있는 백 엔드 서비스를 제공합니다. 일반적으로 이러한 클라이언트 애플리케이션은 봇입니다. 기술 자료를 봇에서 사용할 수 있도록 하려면 HTTP를 통해 액세스할 수 있는 서비스로 게시해야 합니다. 그런 다음, Azure Bot Service를 통해 기술 자료를 사용하여 사용자 질문에 답변하는 봇을 만들고 호스트할 수 있습니다.

1. 왼쪽 패널에서 **기술 자료 배포**를 선택합니다.
1. 페이지 상단에서 **배포**를 선택합니다. 프로젝트를 배포할지 묻는 대화 상자가 표시됩니다. **배포**를 선택합니다.

 ![기술 자료를 배포합니다.](media/create-a-bot/deploy-knowledge-base.png)

1. 서비스가 배포된 후 **봇 만들기**를 선택합니다. 그러면 Azure 구독에서 Web App Bot을 만들 수 있도록 새 브라우저 탭에서 Azure Portal이 열립니다.
1. Azure Portal에서 **웹앱 봇**을 만듭니다. (템플릿의 원본이 신뢰할 수 검사 경고 메시지가 표시 될 수 있습니다. 해당 메시지에 대해 아무 작업도 수행할 필요가 없습니다.) 다음 설정을 업데이트하여 계속합니다.

    - **프로젝트 세부 정보**
        - **구독**: ‘Azure 구독’
        - **리소스 그룹**: *QnA Maker 리소스가 포함된 리소스 그룹*
    - **인스턴스 세부 정보**
        - **위치**: 언어 서비스와 동일한 위치입니다.**
    - **Azure Bot**
        - 봇 핸들: 봇의 고유한 이름
    - 가격 책정 계층 선택
        - **가격 책정 계층**: 무료(F0) (변경 플랜*을 선택*해야 할 수 있음)
    - Microsoft 앱 ID
        - **생성 유형**: ***새 사용자 할당 관리 ID 만들기*** 를 선택합니다. 

5. 설정 업데이트를 계속하려면 **다음**을 선택합니다. 
    - **App Service**
        - **앱 이름**: ***.azurewebsites.net**이 자동으로 추가된 **봇 핸들**과 동일*
        - **SDK 언어**: *C# 또는 Node.js 중 하나 선택*
    - **App Service 계획**
        - **생성 유형**: ***새 App Service 요금제 만들기*** 를 선택합니다.
    - **앱 설정**
        - **언어 리소스 키**: *언어 리소스 키를 복사하여 여기에 붙여넣어야 합니다*:
            - 다른 브라우저 탭을 열고 [https://portal.azure.com](https://portal.azure.com?azure-portal=true)의 Azure Portal로 이동합니다.
            - 언어 서비스 리소스를 찾습니다.
            - **키 및 엔드포인트** 페이지에서 키 중 하나를 복사합니다.
            - 여기에 붙여넣으세요.
        - **언어 프로젝트 이름**: MargiesTravel
        - **언어 서비스 엔드포인트 호스트 이름**: *언어 서비스 엔드포인트로 미리 채워집니다.*
    - **언어 서비스 세부 정보**
        - subscription-id를 구독 ID로 바꿉니다.
        - <resource-group>을 리소스 그룹 리소스 그룹 이름으로 바꿉니다.
        - **계정 이름**: *리소스 이름으로 미리 채워져 있습니다.*

1. **만들기**를 실행합니다. 그런 다음 봇이 작성될 때까지 기다립니다(기다리는 동안 오른쪽 위의 종 모양 알림 아이콘이 애니메이션으로 표시됨). 그런 다음 배포가 완료되었다는 알림에서 **리소스로 이동**을 선택합니다(홈 페이지에서 **리소스 그룹**을 클릭하고 봇을 만든 리소스 그룹을 연 다음 **Azure 봇** 리소스를 선택해도 됨).
1. 봇의 왼쪽 창에서 **설정**을 찾아 **웹 채팅에서 테스트**를 선택하고 봇에 **Hello 및 Welcome** 메시지가 표시될 때까지 기다립니다(초기화하는 데 몇 초 정도 걸릴 수 있음).
1. 테스트 채팅 인터페이스를 사용하여 봇이 기술 자료에서 예상대로 질문에 답변하는지 확인합니다. 예를 들어, `I need to cancel my hotel` 제출을 시도합니다.

봇으로 실험해 보세요. FAQ에서 질문에 매우 정확하게 답변할 수 있지만 학습되지 않은 질문을 해석하는 기능은 제한적이라는 것을 알게 될 것입니다. 항상 Language Studio를 사용하여 기술 자료를 편집하여 개선하고 다시 게시할 수 있습니다.

## 정리

실습이 완료되면  더 이상 필요하지 않은 리소스를 삭제합니다. 이렇게 하면 불필요한 비용이 발생하는 것을 방지할 수 있습니다.

1. [Azure Portal]( https://portal.azure.com)을 열고 만든 리소스가 포함된 리소스 그룹을 선택합니다. 
1. 리소스를 선택하고 **삭제**를 선택한 다음 **예**를 선택하여 확인합니다. 그러면 리소스가 삭제됩니다.

## 자세한 정보

- 질문 답변 서비스에 대한 자세한 정보는 [설명서](https://docs.microsoft.com/azure/cognitive-services/language-service/question-answering/overview)를 참조하세요.
- Microsoft Bot Service에 대해 자세히 알아보려면 [Azure Bot Service 페이지](https://azure.microsoft.com/services/bot-service/)를 참조하세요.
