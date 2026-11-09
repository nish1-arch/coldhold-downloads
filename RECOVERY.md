# Recovering access and interrupted payments

Use Coldhold's local application for passwords and recovery phrases. Never enter them into a website, GitHub issue, chat or support message. A USB flash drive is not a hardware wallet; keep the recovery phrase separately and offline.

## If a payment is pending or unknown

Open **Operations** and select the saved transaction. Check its status and the explorer link for the selected network. If **Retry same transaction** is available, it reuses the saved signed bytes. It does not create a second payment or ask for another signature. Restarting the app preserves the operation.

A new transfer is a new payment. Reconcile the old result before preparing a replacement. Removing the USB cannot cancel a signature that has already been released or sent.

## If the original USB works

Unlock with that USB and your password. Under **Settings**, use **Change password** or **Bind another USB** as needed. Keep the previous USB secret and backups until the new combination has been checked. Old vault backups still require their original password and USB material; a password change cannot remotely revoke them.

## If the USB is lost or the encrypted vault is damaged

In **Settings**, select **Restore with recovery phrase**, connect a replacement physical USB drive, and enter your original phrase and a new password locally. The app checks the recorded addresses before replacing the encrypted vault and keeps a private backup of its original bytes. An unrelated phrase is rejected. It does not format the drive or delete unrelated files.

If the account metadata is missing or damaged, the app shows a recovery notice. First try unlocking with the existing USB and password: successful authentication rebuilds the account list. If you cannot unlock and the recorded address list is unavailable, preserve the old profile and recover separately as described below.

## Recover into a separate profile

Use this only when the usual recovery cannot verify the recorded addresses. Keep the entire original profile so its transaction journal and backups remain available.

1. Close Coldhold completely.
2. On Windows, enter `%APPDATA%` in File Explorer's address bar. On macOS, choose Finder → Go → Go to Folder and enter `~/Library/Application Support`.
3. Rename the existing `Coldhold` folder to a unique backup name, such as `Coldhold-backup-2026-11-09`. Keep the whole folder, including every database and any WAL files. Do not delete individual files.
4. Open Coldhold. Connect your USB, select at least the original number of accounts, and choose **I have a recovery phrase**. Enter the original phrase locally and choose a new password.
5. Compare the resulting public receiving addresses with addresses you already know belong to your wallet. Preserve the original profile backup.
6. Check any old pending/unknown transfers before sending again. The fresh profile cannot know about signatures stored in the old profile; use its preserved journal and the correct network's explorer to reconcile them.

An arbitrary valid phrase is not proof that you restored the original wallet. If you cannot establish the correct addresses, do not make a replacement payment from the new profile. Coldhold cannot recover a lost phrase from a wallet address.

## Updating the application

Close Coldhold, then run the new Windows installer or replace the Mac application. The wallet profile is stored separately from the installation. The Windows uninstaller preserves it. Always retain your offline recovery phrase and profile backup. There is no automatic updater.

Version 0.4.2 has an automated Windows upgrade check from 0.4.0, using an artificial profile marker. It is not a physical USB or real-wallet migration test. Physical Windows USB behavior and a separate Windows 10/11 manual session still require validation.
