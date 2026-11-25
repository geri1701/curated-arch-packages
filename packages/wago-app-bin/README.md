# Upgrading from Wago App 2.14.1 to 3.0.2

Quit Wago App completely before upgrading. Keep the old package and back up
`${XDG_CONFIG_HOME:-$HOME/.config}/wago-app` and the `Interface/AddOns` and
`WTF` directories of the configured WoW installations before starting 3.0.2.
Wago migrates its profile on first launch. If you roll back, quit Wago, set
aside the 3.x profile, reinstall the old package and restore the saved 2.x
profile before starting it. Restore the WoW directories if needed.

An offline Arch test with a synthetic profile verified that WoW paths, app
settings, add-on preferences and the WeakAura install queue survive the
upgrade and a restart. The test files in `Interface/AddOns` and `WTF` were
unchanged, and downgrade with the saved profile worked. Account login and
online sync were not tested.
