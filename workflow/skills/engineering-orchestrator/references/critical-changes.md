# Critical changes

For security, credentials, permissions, privacy, migrations, destructive operations, money, public APIs, production and releases.

- **Authorization is specific.** A request to rotate, migrate, publish or delete authorizes that action only when its exact target is unambiguous. Otherwise ask for that decision. Authority over routine commits does not carry over.
- **Have a way back** before you act: a rollback, a backup you checked, or a containment plan. Say which.
- **Verify as strongly as practical**, on the real target where you are allowed to, and say what could not be verified.

## Credentials

Never request, print, log or return a secret's value.

Before rotating a credential, identify: the service and environment, every consumer, the order of rotation, how the new value is stored and distributed safely, when the old one is revoked, and how to recover if a consumer breaks.

## Persisted data and migrations

Include representative old states in verification: data saved by earlier versions, and features that were turned off. Name the fixtures and the expected result for each.

## External integrations

Keep mocked behavior apart from the real configuration, callbacks and service. Run live checks only inside the user's authorization and with permitted resources. When a live check isn't available, the integration is `unverified`.
