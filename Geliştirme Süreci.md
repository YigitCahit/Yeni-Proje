| Adım | İçerik                                     | Konum                       |
| ---- | ------------------------------------------ | --------------------------- |
| 1    | Veritabanı ve migration hazır              | `models/`, `migrations/`    |
| 2    | Giriş/çıkış veri şemaları (alan doğrulama) | `modules/<modül>/schemas/`  |
| 3    | İş kuralları ve veritabanı işlemleri       | `modules/<modül>/services/` |
| 4    | Servislerin birim testleri                 | `tests/<modül>/services/`   |
| 5    | Rotalar (ince katman)                      | `modules/<modül>/routes/`   |
| 6    | Rota (entegrasyon) testleri                | `tests/<modül>/routes/`     |

```
modules/
  <modül>/
    __init__.py        # blueprint tanımı ve rotaların kaydı
    schemas/           # dosya adları services/routes ile paralel
    services/
    routes/
```

#### Model → Modül Eşlemesi

|Modül|Modeller|
|---|---|
|`auth`|`user.py`, `email_token.py`, `user_session.py`, `session_refresh_token.py`|
|`two_factor`|`user_two_factor.py`, `user_recovery_code.py`|
|`users`|`user.py` (profil), `user_trust_score.py` (okuma)|
|`blocks`|`user_block.py`|
|`kyc`|`kyc_submission.py`|
|`categories`|`category.py`|
|`items`|`item.py`, `item_image.py`, `item_ownership_history.py`|
|`properties`|`property_type.py`, `property_attribute.py`, `property_attribute_option.py`, `property_attribute_value.py`, `property_detail.py`|
|`credits`|`credit_transaction.py`, `credit_purchase.py`, `credit_refund.py`|
|`packages`|`credit_package.py`, `promo_code.py`, `promo_redemption.py`|
|`payments`|`payment_webhook_event.py`|
|`trades`|`trade.py`, `trade_event.py`|
|`messages`|`message.py`|
|`notifications`|`notification.py`, `notification_preference.py`, `push_token.py`|
|`reports`|`report.py`, `report_message_evidence.py`|
|`moderation`|`user_sanction.py`, `user_warning.py`|
|`support`|`support_ticket.py`, `support_ticket_reply.py`|
|`trust`|`trust_score_event.py`, `user_trust_score.py`|
|`admin`|`audit_log.py`, `setting.py`, `setting_change.py`|
### Modüllere Göre `services` ve `routes` Dosyaları

#### 1. `auth`

|`services/`|`routes/`|
|---|---|
|`registration.py`|`register.py`|
|`login.py`|`login.py`|
|`tokens.py` (access token üretim/doğrulama)||
|`refresh.py` (rotation, reuse detection)|`refresh.py`|
|`logout.py`|`logout.py`|
|`password.py` (sıfırlama, değiştirme)|`password.py`|
|`email_verification.py`|`email.py` (doğrulama, yeniden gönderme, adres değiştirme)|
|`access_control.py` (kısıtlı kapsam, allowlist)|
#### 2. `two_factor`

|`services/`|`routes/`|
|---|---|
|`setup.py`|`setup.py`|
|`verification.py`|`verify.py`|
|`recovery_codes.py`|`recovery.py`|
|`enforcement.py` (staff zorunluluğu)|`disable.py`|

#### 3. `sessions`

|`services/`|`routes/`|
|---|---|
|`listing.py`|`sessions.py`|
|`revocation.py`||
|`cleanup.py`||

#### 4. `users`

|`services/`|`routes/`|
|---|---|
|`profile.py`|`me.py`|
|`public_profile.py`|`public_profile.py`|
|`search.py`|`search.py`|
|`account_deletion.py`|`account_deletion.py`|

#### 5. `blocks`

|`services/`|`routes/`|
|---|---|
|`blocking.py` (engelleme + takas iptali)|`blocks.py`|
|`visibility.py` (merkezi iki yönlü filtre)||

#### 6. `kyc`

|`services/`|`routes/`|
|---|---|
|`submission.py`|`submission.py`|
|`review.py`|`admin_review.py`|
|`documents.py` (imzalı erişim, silme)|`admin_documents.py`|
|`eligibility.py` (yayın için KYC kontrolü)||

#### 7. `categories`

|`services/`|`routes/`|
|---|---|
|`category_tree.py`|`categories.py`|
|`category_admin.py`|`admin_categories.py`|

#### 8. `items`

|`services/`|`routes/`|
|---|---|
|`item_crud.py`|`items.py`|
|`publishing.py` (kredi kesimi, durum geçişi)|`publish.py`|
|`tradability.py`|`tradable.py`|
|`images.py`|`images.py`|
|`listing.py` (listeleme, filtre, arama)|`listing.py`|
|`ownership.py` (devir, sıfırlama)||
|`history.py`|`history.py`|
|`status_transitions.py`||

#### 9. `properties`

|`services/`|`routes/`|
|---|---|
|`types.py`|`property_types.py`|
|`attributes.py`|`admin_property_types.py`|
|`values.py` (doğrulama, yazma)|`property_values.py`|
|`details.py` (ortak alanlar, şifreli blok)|`property_details.py`|
|`identifiers.py` (HMAC, mükerrer ilan)||
|`registration_verification.py`|`admin_verification.py`|
|`snapshot.py`||

#### 10. `credits`

|`services/`|`routes/`|
|---|---|
|`pricing.py` (kredi hesabı)|`quote.py`|
|`ledger.py`|`transactions.py`|
|`balance.py`|`balance.py`|
|`publish_fee.py`||
|`purchases.py`|`purchases.py`|
|`refunds.py`|`admin_refunds.py`|
|`reconciliation.py`||

#### 11. `packages`

|`services/`|`routes/`|
|---|---|
|`packages.py`|`packages.py`|
|`promo_validation.py`|`promo.py`|
|`promo_redemption.py`||
|`package_admin.py`|`admin_packages.py`|
|`promo_admin.py`|`admin_promo.py`|

#### 12. `payments`

|`services/`|`routes/`|
|---|---|
|`webhook_ingest.py` (imza, kayıt)|`webhooks.py`|
|`webhook_processor.py`|`admin_webhooks.py` (yeniden işleme)|
|`providers/` (sağlayıcıya özel alt dizin)||
|`provider_verification.py`||

#### 13. `trades`

|`services/`|`routes/`|
|---|---|
|`offers.py`|`offers.py`|
|`value_rules.py` (değer farkı, tür kuralı)||
|`acceptance.py`|`accept.py`|
|`rejection.py`|`reject.py`|
|`cancellation.py`|`cancel.py`|
|`confirmation.py`|`confirm.py`|
|`completion.py` (sahiplik değişimi)||
|`expiry.py` (süre dolumu)||
|`snapshots.py`||
|`events.py`|`trade_queries.py` (liste, detay, geçmiş)|
|`queries.py`|`item_trades.py`|

#### 14. `messages`

|`services/`|`routes/`|
|---|---|
|`permissions.py` (gönderme izni)||
|`sending.py`|`messages.py`|
|`conversations.py`|`conversations.py`|
|`read_state.py`|`read.py`|
|`retention.py`||
|`realtime.py`|`websocket.py`|

#### 15. `notifications`

|`services/`|`routes/`|
|---|---|
|`dispatcher.py` (merkezi üretim)||
|`queries.py`|`notifications.py`|
|`preferences.py`|`preferences.py`|
|`push.py`|`push_tokens.py`|
|`email_delivery.py`||
|`cleanup.py`||

#### 16. `reports`

|`services/`|`routes/`|
|---|---|
|`creation.py`|`reports.py`|
|`evidence.py`||
|`queue.py`|`mod_reports.py`|
|`resolution.py`|`mod_resolution.py`|

#### 17. `moderation`

|`services/`|`routes/`|
|---|---|
|`sanctions.py`|`mod_sanctions.py`|
|`sanction_expiry.py`||
|`warnings.py`|`mod_warnings.py`|
|`my_status.py` (kullanıcının kendi yaptırım/uyarı görünümü)|`my_status.py`|
|`item_moderation.py`|`mod_items.py`|

#### 18. `support`

|`services/`|`routes/`|
|---|---|
|`tickets.py`|`tickets.py`|
|`replies.py`|`replies.py`|
|`assignment.py`|`mod_tickets.py`|
|`escalation.py`||
|`auto_close.py`||

#### 19. `trust`

|`services/`|`routes/`|
|---|---|
|`events.py` (merkezi olay yazımı)||
|`calculator.py`||
|`recalculation.py`|`admin_trust.py`|
|`queries.py`|`trust.py`|
|`abuse_filter.py`||

#### 20. `admin`

|`services/`|`routes/`|
|---|---|
|`audit.py` (merkezi denetim kaydı)|`audit_logs.py`|
|`settings.py`|`settings.py`|
|`user_admin.py`|`users.py`|
|`item_admin.py`|`items.py`|
|`trade_admin.py`|`trades.py`|
|`message_access.py` (gerekçeli okuma)|`message_access.py`|
### `common/` İçinde Kalması Gerekenler

|Dosya|Amaç|
|---|---|
|`errors.py`|Ortak hata formatı|
|`decorators.py`|Kimlik, rol, kısıtlı kullanıcı kontrolü|
|`pagination.py`|Sayfalama|
|`config_reader.py`|`.env` ve ayar okuma (tek erişim noktası)|
|`crypto.py`|Şifreleme ve HMAC yardımcıları|
|`ip_utils.py`|IP dönüşümü, gerçek IP|
|`idempotency.py`|Tekrar koruması|
|`time_utils.py`|UTC yardımcıları|
|`constants.py`|Olay adları, bildirim türleri, ayar anahtarları|
### Arka Plan Görevleri (`tasks/`)

Servislerdeki `expiry`, `cleanup`, `retention`, `reconciliation`, `recalculation` dosyaları görev olarak çağrılır. Görev dosyaları da modüle göre ayrılmalı: `tasks/trades.py`, `tasks/messages.py`, `tasks/notifications.py`, `tasks/credits.py`, `tasks/trust.py`, `tasks/support.py`, `tasks/moderation.py`. Görev dosyaları yalnızca zamanlama ve yeniden deneme içerir, iş mantığı servislerde kalır.