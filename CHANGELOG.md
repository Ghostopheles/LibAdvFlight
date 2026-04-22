# v1.1.1
- Bumped TOC
- Added some performance optimizations to prevent wasteful state checking
    - As a result of these optimizations, the following events may fire alongside `PLAYER_ENTERING_WORLD`, since we now check the current state on login/zone transitions
        - `ADV_FLYING_ENABLED`, `ADV_FLYING_ENABLE_STATE_CHANGED`
        - `ADV_FLYING_START`, `ADV_FLYING_STATE_CHANGED`
    - This shouldn't require any actions from addon authors and should just work(tm)
    - The following events may fire twice simultaneously due to malicious double-firing Blizzard events:
        - `ADV_FLYING_START`, `ADV_FLYING_END`, `ADV_FLYING_STATE_CHANGED`