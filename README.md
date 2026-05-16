# OneView Server Tag

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![GitHub issues](https://img.shields.io/github/issues/oneviewhub/gtm-server-tag.svg)](https://github.com/oneviewhub/gtm-server-tag/issues)


This template functions as a **Server Tag** within your GTM Server-side container, processing incoming HTTP requests and forwarding enriched event data to OneView's attribution and conversion tracking system.

## 📋 Prerequisites

* **Google Tag Manager Server-side container** (running on GCP or your preferred platform)
* **OneView account** with API access

## 🛠️ Installation

1. Open your **GTM Server-side container** (not web container)
2. Navigate to **Templates** > **Tag Templates**
3. Click **Search Gallery**
4. Search for "OneView Server Tag"
5. Click **Add to workspace**

## ⚙️ Configuration

### Required Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **API Key** | Your OneView API key from the dashboard | ✅ |

### Optional Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| **Idempotency Key** | Prevents duplicate events with the same key | - |
| **User Identifiers** | Additional user identification data | Auto-detected |
| **Consent Settings** | Override consent mode settings | Auto-detected |

### User Identifiers

The template automatically extracts user identifiers from GTM Common Event Data:

- 💻 **Client ID**: Google Analytics Client ID (`client_id`)
- 👤 **User ID**: Your internal user identifier (`user_id`)
- 🏢 **Organization ID**: Company or organization identifier
- 📧 **Email Address**: User's email from `user_data.email_address` (automatically hashed)
- 📞 **Phone Number**: E.164 format phone number from `user_data.phone_number` (automatically hashed)

> **Note**: When using GA4 Client in your server container, user identifiers are automatically included via Common Event Data. Manual configuration is only needed for additional identifiers.

## 🔒 Privacy & Consent Management

### Automatic PII Protection

- **Email addresses** are SHA-256 hashed before transmission
- **Phone numbers** are hashed in both E.164 and numeric formats
- **Names** (first/last) are hashed and removed from plaintext
- **Plaintext PII** is automatically omitted from requests

### Google Consent Mode Support

- Automatically detects Google Consent Mode (both v1 and v2) signals
- **ad_user_data**: Controls sending hashed PII to media partners
- **ad_personalization**: Controls personalized advertising consent
- 🇪🇺 **EU compliance**: Consent denied by default, PII automatically hashed

## 🐛 Debugging & Monitoring

### Enable Debug Mode

1. Set your GTM Server container to **Debug/Preview** mode
2. Check the **Console** tab in your server container preview
3. Look for `OneViewEvent` log entries with request/response details


### Common Issues

| Issue | Solution |
|-------|----------|
| **No events firing** | Verify your server container is receiving requests from web container |
| **Duplicate events** | Use idempotency keys for critical events |
| **Missing Common Event Data** | Ensure GA4 Client is properly configured in server container |
| **PII not hashed** | Ensure email/phone formats are valid in incoming data |
| **Consent not detected** | Check consent mode implementation in web container |

### Reporting Issues

- Use the [GitHub Issues](https://github.com/oneviewhub/gtm-server-tag/issues) page
- Include GTM container details and error logs
- Provide steps to reproduce the issue

## 📝 License

This project is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

- **Documentation**: [OneView Documentation](https://oneviewhub.com/docs)
- **Support**: [OneView Support](https://oneviewhub.com/docs)

## 🏷️ Changelog

### v2.1.1
- Updated email canonicalization logic

### v2.0.0
- Updated to use API v2

### v1.2.3
- Added advanced email parsing to properly format edge cases
- Fixed email canonicalization bug
- Updated UI descriptions

### v1.1.0
- Ability to set explicit `ad_storage` and `analytics_storage` permissions from UI

### v1.0.1
- Fixed mismatched hashes

### v1.0.0
- Initial release
- Support for Google Consent Mode v1 & v2
- Automatic PII hashing and privacy compliance
- Multiple user identifier types
- Idempotency key support
- Comprehensive logging and debugging

---

Made with ❤️ by the OneView team
