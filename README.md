# Stormbound

A browser-based wizard arena game. Progress is saved locally by default. Firebase can be configured to enable Google, Facebook, and Apple sign-in with cloud saves across devices.

## Enable sign-in and cloud saves

1. Create a Firebase project, add a Web app, and copy its web configuration into `multiplayer-config.js` under `STORMBOUND_FIREBASE_CONFIG`. The web config is public; do not put provider client secrets in this file.
2. In Firebase Authentication, enable Google, Facebook, and Apple under Sign-in providers. Each provider also needs its own client credentials and callback configuration in its provider console.
3. Add the deployed game domain and any local development domain to Firebase Authentication's authorized domains.
4. Create a Cloud Firestore database and publish these rules so each signed-in player can access only their own save:

```text
rules_version = '2';
service cloud.firestore {
	match /databases/{database}/documents {
		match /users/{userId}/saves/{saveId} {
			allow read, write: if request.auth != null && request.auth.uid == userId;
		}
	}
}
```

Cloud saves merge coins, unlocked spells, owned avatars and pets, and quest progress with the current device's local save. Without Firebase configuration, the game continues saving locally and the sign-in providers remain disabled.
