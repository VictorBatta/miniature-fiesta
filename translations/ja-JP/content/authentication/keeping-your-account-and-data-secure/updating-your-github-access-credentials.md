---
title: GitHub アクセス認証情報を更新する
intro: '{% data variables.product.product_name %} 認証情報は、{% ifversion not ghae %}パスワードだけではなく、{% endif %}{% data variables.product.product_name %} に伝達するのに使うアクセストークン、SSH キーおよびアプリケーション API トークンを含みます。 必要があれば、すべてのアクセス認証情報をリセットできます。'
redirect_from:
  - /articles/rolling-your-credentials/
  - /articles/how-can-i-reset-my-password/
  - /articles/updating-your-github-access-credentials
  - /github/authenticating-to-github/updating-your-github-access-credentials
  - /github/authenticating-to-github/keeping-your-account-and-data-secure/updating-your-github-access-credentials
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
topics:
  - Identity
  - Access management
shortTitle: Update access credentials
---

{% ifversion not ghae %}
## 新しいパスワードをリクエストする

1. To request a new password, visit {% ifversion fpt or ghec %}https://{% data variables.product.product_url %}/password_reset{% else %}`https://{% data variables.product.product_url %}/password_reset`{% endif %}.
2. Enter the email address associated with your account on {% ifversion ghae %}{% data variables.product.product_name %}{% else %}{% data variables.product.product_location %}{% endif %}, then click **Send password reset email.** The email will be sent to the backup email address if you have one configured. ![パスワードリセットのメールリクエストダイアログ](/assets/images/help/settings/password-recovery-email-request.png)
3. パスワードをリセットするためのリンクがメールで届きます。 メールを受信してから 3 時間以内に、このリンクをクリックする必要があります。 弊社からメールが届かない場合、スパムフォルダを確認してください。
4. If you have enabled two-factor authentication, you will be prompted for your 2FA credentials. Type your 2FA credentials or one of your 2FA recovery codes and click **Verify**. ![Two-factor authentication prompt](/assets/images/help/2fa/2fa-password-reset.png)
5. Type a new password, confirm your new password, and click **Change password**. For help creating a strong password, see "[Creating a strong password](/articles/creating-a-strong-password)."
  {% ifversion fpt or ghec %}![Password recovery box](/assets/images/help/settings/password-recovery-page.png){% else %}
  ![パスワードリカバリボックス](/assets/images/enterprise/settings/password-recovery-page.png){% endif %}

{% tip %}

To avoid losing your password in the future, we suggest using a secure password manager, like [LastPass](https://lastpass.com/) or [1Password](https://1password.com/).

{% endtip %}

## 既存のパスワードを変更する

{% data reusables.repositories.blocked-passwords %}

1. {% data variables.product.product_name %} への {% data variables.product.signin_link %}
{% data reusables.user_settings.access_settings %}
{% data reusables.user_settings.security %}
4. [Change password] の下で、古いパスワード、新しい強靭なパスワードを入力し、新しいパスワードを確認します。 強靭なパスワードを作成するための参考として、「[強靭なパスワードを作成する](/articles/creating-a-strong-password)」を参照してください。
5. [**Update password**] をクリックします。

{% tip %}

セキュリティを強化するために、パスワードの変更に加えて 2 要素認証を有効にしてください。 詳細は「[2 要素認証について](/articles/about-two-factor-authentication)」を参照してください。

{% endtip %}
{% endif %}
## アクセストークンを更新する

アクセストークンのレビューと削除の方法については、「[許可されたインテグレーションをレビューする](/articles/reviewing-your-authorized-integrations)」を参照してください。 新しいアクセストークンを作成するには、「[個人アクセストークンを作成する](/github/authenticating-to-github/creating-a-personal-access-token)」を参照してください。

## SSH キーを更新する

SSH キーのレビューおよび削除については「[SSH キーをレビューする](/articles/reviewing-your-ssh-keys)」を参照してください。 新しい SSH キーの生成および追加については、「[SSH キーを生成する](/articles/generating-an-ssh-key)」を参照してください。

## API トークンをリセットする

{% data variables.product.product_name %} に登録したアプリケーションがある場合、OAuthトークンのリセットを考えることになります。 詳しい情報については、「[認証をリセットする](/rest/reference/apps#reset-an-authorization)」エンドポイントを参照してください。

{% ifversion not ghae %}
## 許可されていないアクセスを防止する

アカウントを保護し権限のないアクセスを防止するためのさらなるヒントについては、「[許可されていないアクセスを防止する](/articles/preventing-unauthorized-access)」を参照してください。
{% endif %}
