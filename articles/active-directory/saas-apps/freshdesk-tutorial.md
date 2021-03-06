---
title: 教學課程：Azure Active Directory 與 FreshDesk 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 FreshDesk 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2018
ms.author: jeedes
ms.openlocfilehash: b5968b83fc9beb481e2ad2c0cd44d2c284747fa1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51010808"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>教學課程：Azure Active Directory 與 FreshDesk 整合

在本教學課程中，您會了解如何整合 FreshDesk 與 Azure Active Directory (Azure AD)。

FreshDesk 與 Azure AD 整合提供下列優點：

- 您可以在 Azure AD 中控制可存取 FreshDesk 的人員。
- 您可以讓使用者使用他們的 Azure AD 帳戶自動登入 FreshDesk (單一登入)。
- 您可以在 Azure 入口網站中集中管理您的帳戶。

如果您想要了解有關 SaaS 應用程式與 Azure AD 之整合的更多詳細資料，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>必要條件

若要設定與 FreshDesk 的 Azure AD 整合，您需要下列項目：

- Azure AD 訂用帳戶
- 啟用 FreshDesk 單一登入的訂用帳戶

> [!NOTE]
> 若要測試本教學課程中的步驟，我們不建議使用生產環境。

若要測試本教學課程中的步驟，您應該遵循這些建議：

- 除非必要，否則請勿使用生產環境。
- 如果您沒有 Azure AD 試用環境，您可以[取得一個月試用](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中測試 Azure AD 單一登入。 本教學課程中說明的案例由二項主要的基本工作組成：

1. 從資源庫新增 FreshDesk
2. 設定並測試 Azure AD 單一登入

## <a name="adding-freshdesk-from-the-gallery"></a>從資源庫新增 FreshDesk

若要設定 FreshDesk 與 Azure AD 整合，您需要從資源庫將 FreshDesk 新增至受控 SaaS 應用程式清單。

**若要從資源庫新增 FreshDesk，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory] 圖示。 

    ![Azure Active Directory 按鈕][1]

2. 瀏覽至 [企業應用程式]。 然後移至 [所有應用程式]。

    ![企業應用程式刀鋒視窗][2]

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式] 按鈕。

    ![新增應用程式按鈕][3]

4. 在搜尋方塊中，輸入 **FreshDesk**，從結果面板中選取 [FreshDesk]，然後按一下 [新增] 按鈕以新增應用程式。

    ![結果清單中的 FreshDesk](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 "Britta Simon" 的測試使用者身分，使用 FreshDesk 設定及測試 Azure AD 單一登入。

若要讓單一登入運作，Azure AD 必須知道 FreshDesk 與 Azure AD 中互相對應的使用者。 換句話說，必須建立 Azure AD 使用者和 FreshDesk 中相關使用者之間的連結關聯性。

若要設定及測試與 FreshDesk 搭配運作的 Azure AD 單一登入，您需要完成下列建置組塊：

1. **[設定 Azure AD 單一登入](#configuring-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[建立 Azure AD 測試使用者](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
3. **[建立 FreshDesk 測試使用者](#creating-a-freshdesk-test-user)** - 讓 FreshDesk 中對應的 Britta Simon 連結到使用者在 Azure AD 中的代表項目。
4. **[指派 Azure AD 測試使用者](#assigning-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[測試單一登入](#testing-single-sign-on)** - 驗證組態是否能運作。

### <a name="configuring-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入，並在您的 FreshDesk 應用程式中設定單一登入。

**若要設定與 FreshDesk 搭配運作的 Azure AD 單一登入，請執行下列步驟：**

1. 在 Azure 入口網站的 [FreshDesk] 應用程式整合頁面上，按一下 [單一登入]。

    ![設定單一登入連結][4]

2. 在 [選取單一登入方法] 對話方塊上，按一下 [SAML] 模式的 [選取]，啟用單一登入。

    ![設定單一登入](common/tutorial_general_301.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。

    ![設定單一登入](common/editconfigure.png)

4. 在 [基本 SAML 組態] 區段上，執行下列步驟：

    ![FreshDesk 網域與 URL 單一登入資訊](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    a. 在 [登入 URL] 文字方塊中，以下列模式輸入 URL：`https://<tenant-name>.freshdesk.com` 或 Freshdesk 所建議的任何其他值。

    b. 在 [識別碼 (實體識別碼)] 文字方塊中，以下列模式輸入 URL：`https://<tenant-name>.freshdesk.com` 或 Freshdesk 所建議的任何其他值。

    > [!NOTE]
    > 這些都不是真正的值。 使用實際的「登入 URL」及「識別碼」來更新這些值。 請連絡 [FreshDesk 用戶端支援小組](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg)以取得這些值。

5. FreshDesk 應用程式需要特定格式的 SAML 判斷提示，需要您加入自訂屬性對應到您的 SAML 權杖屬性設定。 以下螢幕擷取畫面顯示上述的範例。 [使用者識別碼] 的預設值是 **user.userprincipalname**，但是 **FreshDesk** 會預期它是與使用者電子郵件地址對應的值。 對此您可以使用清單中的 **user.mail** 屬性，或者根據組織組態使用適當的屬性值。

    ![映像](./media/freshdesk-tutorial/i4-attribute.png)

6. 在 [使用者屬性與宣告] 對話方塊的 [使用者宣告] 區段中，如上圖所示設定 SAML 權杖屬性，然後執行下列步驟：
    
    a. 按一下 [編輯] 圖示以開啟 [管理使用者宣告] 對話方塊。

    ![映像](./media/freshdesk-tutorial/i2-attribute.png)

    ![映像](./media/freshdesk-tutorial/i3-attribute.png)

    b. 從 [來源屬性] 清單中，選取 [user.mail]。

    c. 按一下 [檔案] 。

7. 在 [SAML 簽署憑證] 頁面上的 [SAML 簽署憑證] 區段中，按一下 [下載] 以下載**憑證 (Base64)**，然後將憑證檔案儲存在電腦上。

    ![憑證下載連結](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png)

8. 開啟 [命令提示字元] 並執行下列命令：

    a. 在命令提示字元中輸入 `certutil.exe -dump FreshDesk.cer` 值。

    > [!NOTE]
    > 這裡的 **FreshDesk.cer** 是您從 Azure 入口網站下載的憑證。

    b. 複製 [Cert 雜湊(sha256)] 值並貼到 [記事本] 中。 

9. 在 [設定 FreshDesk] 區段上，依據您的需求複製適當的 URL。

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

    ![FreshDesk 組態](common/configuresection.png)

10. 在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 Freshdesk 公司網站。

11. 選取 [設定] 圖示，並且在 [安全性] 區段中執行下列步驟：

    ![單一登入](./media/freshdesk-tutorial/IC776770.png "單一登入")
  
    a. 針對 [單一登入 (SSO)] 選取 [啟用]。

    b. 選取 [SAML SSO] 。

    c. 在 [SAML 登入 URL] 文字方塊中，貼上您從 Azure 入口網站複製的**登入 URL** 值。

    d. 在 [登出 URL] 文字方塊中，貼上您從 Azure 入口網站複製的**登出 URL** 值。

    e. 在 [安全性憑證指紋] 文字方塊中，貼上您先前取得的 [Cert 雜湊(sha256)] 值。
  
    f. 按一下 [檔案] 。

### <a name="creating-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

本節的目標是要在 Azure 入口網站中建立一個名為 Britta Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]、[使用者] 和 [所有使用者]。

    ![建立 Azure AD 使用者][100]

2. 在畫面頂端選取 [新增使用者]。

    ![建立 Azure AD 測試使用者](common/create_aaduser_01.png) 

3. 在 [使用者] 屬性中，執行下列步驟。

    ![建立 Azure AD 測試使用者](common/create_aaduser_02.png)

    a. 在 [名稱] 欄位中，輸入 **BrittaSimon**。
  
    b. 在 [使用者名稱] 欄位中，輸入 **brittasimon@yourcompanydomain.extension**  
    例如， BrittaSimon@contoso.com

    c. 依序選取 [屬性] [顯示密碼] 核取方塊，然後記下 [密碼] 方塊中顯示的值。

    d. 選取 [建立] 。

### <a name="creating-a-freshdesk-test-user"></a>建立 FreshDesk 測試使用者

若要讓 Azure AD 使用者可以登入 FreshDesk，則必須將他們佈建到 FreshDesk。  
FreshDesk 需以手動的方式佈建。

**若要佈建使用者帳戶，請執行下列步驟：**

1. 登入您的 **Freshdesk** 租用戶。

2. 在頂端的功能表中，按一下 [系統管理員] 。

    ![管理](./media/freshdesk-tutorial/IC776772.png "管理")

3. 在 [一般設定] 索引標籤上，按一下 [代理程式]。
  
    ![代理程式](./media/freshdesk-tutorial/IC776773.png "代理程式")

4. 按一下 [新增代理程式] 。

    ![新代理程式](./media/freshdesk-tutorial/IC776774.png "新代理程式")

5. 在 [代理程式資訊] 對話方塊中，執行下列步驟：

    ![代理程式資訊](./media/freshdesk-tutorial/IC776775.png "代理程式資訊")

    a. 在 [電子郵件]  文字方塊中，輸入您要佈建的 Azure AD 帳戶的電子郵件地址。

    b. 在 [全名]  文字方塊中，輸入您要佈建的 Azure AD 帳戶名稱。

    c. 在 [職稱]  文字方塊中，輸入您要佈建的 Azure AD 帳戶的職稱。

    d. 按一下 [檔案] 。

    >[!NOTE]
    >Azure AD 帳戶的持有者會收到一封包含連結的電子郵件，以在啟用帳戶前進行確認。
    >
    >[!NOTE]
    >您可以使用任何其他的 Freshdesk 使用者帳戶建立工具或 Freshdesk 提供的 API 將 AAD 使用者帳戶佈建至 Freshdesk。

### <a name="assigning-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會把 FreshDesk 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式] 和 [所有應用程式]。

    ![指派使用者][201]

2. 在應用程式清單中，選取 [FreshDesk] 。

    ![設定單一登入](./media/freshdesk-tutorial/tutorial_freshdesk_app.png)

3. 在左側功能表中，按一下 [使用者和群組]。

    ![指派使用者][202]

4. 按一下 [新增] 按鈕。 然後選取 [新增指派] 對話方塊上的 [使用者和群組]。

    ![指派使用者][203]

5. 在 [使用者和群組] 對話方塊的 [使用者] 清單中，選取 [Britta Simon]，然後按一下畫面底部的 [選取] 按鈕。

6. 在 [新增指派] 對話方塊中，選取 [指派] 按鈕。

### <a name="testing-single-sign-on"></a>測試單一登入

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 FreshDesk 圖格時，您應該會自動登入您的 FreshDesk 應用程式。
如需存取面板的詳細資訊，請參閱[存取面板簡介](../user-help/active-directory-saas-access-panel-introduction.md)。

## <a name="additional-resources"></a>其他資源

* [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](tutorial-list.md)
* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
