---
title: 教學課程：Azure Active Directory 與 Appraisd 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Appraisd 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: db063306-4d0d-43ca-aae0-09f0426e7429
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2018
ms.author: jeedes
ms.openlocfilehash: dae9a59b89f03a50b1adaed55cd8f97c06906526
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2018
ms.locfileid: "49118103"
---
# <a name="tutorial-azure-active-directory-integration-with-appraisd"></a>教學課程：Azure Active Directory 與 Appraisd

在本教學課程中，您將了解如何整合 Appraisd 與 Azure Active Directory (Azure AD)。

Appraisd 與 Azure AD 整合提供下列優點：

- 您可以在 Azure AD 中控制可存取 Appraisd 的人員。
- 您可以讓使用者使用他們的 Azure AD 帳戶自動登入 Appraisd (單一登入)。
- 您可以在 Azure 入口網站中集中管理您的帳戶。

如果您想要了解有關 SaaS 應用程式與 Azure AD 之整合的更多詳細資料，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>必要條件

若要設定與 Appraisd 的 Azure AD 整合，您需要下列項目：

- Azure AD 訂用帳戶
- 已啟用 Appraisd 單一登入的訂用帳戶

> [!NOTE]
> 若要測試本教學課程中的步驟，我們不建議使用生產環境。

若要測試本教學課程中的步驟，您應該遵循這些建議：

- 除非必要，否則請勿使用生產環境。
- 如果您沒有 Azure AD 試用環境，您可以[取得一個月試用](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>案例描述
在本教學課程中，您會在測試環境中測試 Azure AD 單一登入。 本教學課程中說明的案例由二項主要的基本工作組成：

1. 從資源庫新增 Appraisd
2. 設定並測試 Azure AD 單一登入

## <a name="adding-appraisd-from-the-gallery"></a>從資源庫新增 Appraisd
若要設定 Appraisd 與 Azure AD 整合，您需要從資源庫將 Appraisd 新增至受控 SaaS 應用程式清單中。

**若要從資源庫新增 Appraisd，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory] 圖示。 

    ![映像](./media/appraisd-tutorial/selectazuread.png)

2. 瀏覽至 [企業應用程式]。 然後移至 [所有應用程式]。

    ![映像](./media/appraisd-tutorial/a_select_app.png)
    
3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式] 按鈕。

    ![映像](./media/appraisd-tutorial/a_new_app.png)

4. 在搜尋方塊中，輸入 **Appraisd**，從結果面板中選取 ，然後按一下 [新增] 按鈕以新增應用程式。

     ![映像](./media/appraisd-tutorial/tutorial_appraisd_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 "Britta Simon" 的測試使用者身分，使用 Appraisd 設定及測試 Azure AD 單一登入。

若要讓單一登入運作，Azure AD 必須知道 Appraisd 與 Azure AD 中互相對應的使用者。 換句話說，必須建立 Azure AD 使用者和 Appraisd 中相關使用者之間的連結關聯性。

若要設定及測試與 Appraisd 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
3. **[建立 Appraisd 測試使用者](#create-an-appraisd-test-user)** - 在 Appraisd 中建立一個與 Azure AD 使用者代表 Britta Simon 連結的對應使用者。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[測試單一登入](#test-single-sign-on)**，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入，然後在您的 Appraisd 應用程式中設定單一登入。

**若要使用 Appraisd 設定 Azure AD 單一登入，請執行下列步驟：**

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Appraisd] 應用程式整合頁面上，選取 [單一登入]。

    ![映像](./media/appraisd-tutorial/B1_B2_Select_SSO.png)

2. 在 [選取單一登入方法] 對話方塊中，選取 [SAML] 模式以啟用單一登入。

    ![映像](./media/appraisd-tutorial/b1_b2_saml_sso.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 按鈕以開啟 [基本 SAML 組態] 對話方塊。

    ![映像](./media/appraisd-tutorial/b1-domains_and_urlsedit.png)

4. 若您想要以 **IDP** 起始模式設定應用程式，請在 [基本 SAML 組態] 區段執行下列步驟：

    ![映像](./media/appraisd-tutorial/tutorial_appraisd_url.png)

    a. 按一下 [設定額外的 URL]。 

    b. 在 [轉送狀態] 文字方塊中，鍵入 URL：`<TENANTCODE>`

    c. 如果您想要以 **SP** 起始模式設定應用程式，請在 [登入 URL] 文字方塊中使用下列模式輸入 URL：`https://app.appraisd.com/saml/<TENANTCODE>`

    > [!NOTE]
    > 您可在 [Appraisd SSO 組態] 頁面上取得實際的 [登入 URL] 與 [轉送狀態] 值，稍後會在本教學課程中說明該頁面。
    
5. Appraisd 應用程式需要特定格式的 SAML 判斷提示。 設定此應用程式的下列宣告。 您可以在應用程式整合頁面的 [使用者屬性] 區段中，管理這些屬性的值。 按一下 [編輯] 按鈕以開啟 [使用者使屬性] 對話方塊。

    ![映像](./media/appraisd-tutorial/i3-attribute.png)

6. 在 [使用者屬性] 對話方塊的 [使用者宣告] 區段中，如上圖所示設定 SAML 權杖屬性，然後執行下列步驟：
    
    a. 按一下 [編輯] 按鈕以開啟 [管理使用者宣告] 對話方塊。

    ![映像](./media/appraisd-tutorial/i2-attribute.png)

    ![映像](./media/appraisd-tutorial/i4-attribute.png)

    b. 從 [來源屬性] 清單中，選取屬性值。

    c. 按一下 [檔案] 。 

7. 在 [SAML 簽署憑證] 區段中，按一下 [下載] 來下載 [憑證 (Base64)]，然後將它儲存在您的電腦上。

    ![映像](./media/appraisd-tutorial/tutorial_appraisd_certficate.png) 

8. 在 [安裝 Appraisd] 區段上，依據您的需求複製適當的 URL。

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

    ![映像](./media/appraisd-tutorial/d1_samlsonfigure.png)

9. 在不同的網頁瀏覽器視窗中，以安全性系統管理員身分登入 Appraisd。

10. 按一下頁面右上方的 [設定] 圖示，然後導覽至 [組態]。

    ![映像](./media/appraisd-tutorial/tutorial_appraisd_sett.png)

11. 從功能表左側，按一下 [SAML 單一登入]。

    ![映像](./media/appraisd-tutorial/tutorial_appraisd_single.png)

12. 在 [SAML 2.0 單一登入組態] 頁面上，執行下列步驟：
    
    ![映像](./media/appraisd-tutorial/tutorial_appraisd_saml.png)

    a. 複製 [預設轉送狀態] 值，並將它貼到 Azure 入口網站上 [基本 SAML 組態] 的 [轉送狀態] 文字方塊中。

    b. 複製 [服務起始的登入 URL] 值，並將它貼到 Azure 入口網站上 [基本 SAML 組態] 的 [登入 URL] 文字方塊中。

13. 在相同頁面向下捲動到 [識別使用者] 之下，執行下列步驟：

    ![映像](./media/appraisd-tutorial/tutorial_appraisd_identifying.png)

    a. 在 [識別提供者單一登入 URL] 文字方塊中，貼上您從 Azure 入口網站複製的 [登入 URL] 值，然後按一下 [儲存]。

    b. 在 [識別提供者簽發者 URL] 文字方塊中，貼上您從 Azure 入口網站複製的 [Azure AD 識別碼] 值，然後按一下 [儲存]。

    c. 在「記事本」中開啟從 Azure 入口網站下載的 Base-64 編碼憑證，複製其內容，然後貼到 [X.509 憑證] 方塊並按一下 [儲存]。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

本節的目標是要在 Azure 入口網站中建立一個名為 Britta Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]、[使用者]，然後選取 [所有使用者]。

    ![映像](./media/appraisd-tutorial/d_users_and_groups.png)

2. 選取畫面頂端的 [新增使用者]。

    ![映像](./media/appraisd-tutorial/d_adduser.png)

3. 在 [使用者] 屬性中，執行下列步驟。

    ![映像](./media/appraisd-tutorial/d_userproperties.png)

    a. 在 [名稱] 欄位中，輸入 **BrittaSimon**。
  
    b. 在 [使用者名稱] 欄位中，鍵入 **brittasimon@yourcompanydomain.extension**  
    例如， BrittaSimon@contoso.com

    c. 依序選取 [屬性]、[顯示密碼] 核取方塊，然後記下 [密碼] 方塊中顯示的值。

    d. 選取 [建立] 。
 
### <a name="create-an-appraisd-test-user"></a>建立 Appraisd 測試使用者

若要讓 Azure AD 使用者能夠登入 Appraisd，必須將這些使用者佈建到 Appraisd。 在 Appraisd 中，需手動進行佈建。

**若要佈建使用者帳戶，請執行下列步驟：**

1. 以安全性系統管理員身分登入 Appraisd。

2. 按一下頁面右上方的 [設定] 圖示，然後導覽至 [管理中心]。

    ![映像](./media/appraisd-tutorial/tutorial_appraisd_admin.png)

3. 在頁面頂端的工具列中，按一下 [人員]，然後巡覽至 [新增使用者]。

    ![映像](./media/appraisd-tutorial/tutorial_appraisd_user.png)

4. 在 [新增使用者] 頁面上，執行下列步驟：

    ![映像](./media/appraisd-tutorial/tutorial_appraisd_newuser.png)
    
    a. 在 [名字] 文字方塊中，輸入使用者的名字，例如 **Britta**。

    b. 在 [姓氏] 文字方塊中，輸入使用者的姓氏，例如 **Simon**。

    c. 在 [電子郵件] 文字方塊中，輸入使用者的電子郵件，例如 **Brittasimon@contoso.com**。

    d. 按一下 [新增使用者] 。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會把 Appraisd 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]、[所有應用程式]。

    ![映像](./media/appraisd-tutorial/d_all_applications.png)

2. 在應用程式清單中，選取 [Appraisd]。

    ![映像](./media/appraisd-tutorial/tutorial_appraisd_app.png)

3. 在左側功能表中，選取 [使用者和群組]。

    ![映像](./media/appraisd-tutorial/d_leftpaneusers.png)

4. 選取 [新增] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![映像](./media/appraisd-tutorial/d_assign_user.png)

4. 在 [使用者和群組] 對話方塊的 [使用者] 清單中，選取 [Britta Simon]，然後按一下畫面底部的 [選取] 按鈕。

5. 在 [新增指派] 對話方塊中，選取 [指派] 按鈕。
    
### <a name="test-single-sign-on"></a>測試單一登入

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 Appraisd 圖格時，您應會自動登入 Appraisd 應用程式。
如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../active-directory-saas-access-panel-introduction.md)。 

## <a name="additional-resources"></a>其他資源

* [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](tutorial-list.md)
* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)
