---
title: Introduction
description: Learn about the Humanos API and its capabilities
---

# Introduction

The **Humanos API** provides a secure and reliable interface for managing users, identities, and business processes. It incorporates multiple layers of security, including: API key authentication, HMAC-SHA256 signature validation, AES-256-GCM encryption for sensitive data, IP and Domain validations. The API enables organizations to create and manage **users,** **identities and processes.** A **process** is delivered through a unique, secure link where the client can complete all requested interactions by the organization. The **Humanos API** provides real-time notifications, via secure webhooks, to the organization whenever a relevant event is completed.

Base URL: `https://api.humanos.id/v0`

---

A **process** is composed of an ordered sequence of **subprocesses**, such as:

- **Identity Validations (KYC):** Government-issued ID scan combined with a selfie for verification
- **Signature Requests:** Send PDFs for clients to view, sign, or reject
- **Consents:** Collect mandatory and optional consents, with the option to attach external links
- **Form Filling:** Seamlessly create and manage forms

**Note**: API keys, webhook enpoints, secrets and processes are managed through the organization dashboard.
