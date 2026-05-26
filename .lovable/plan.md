## Problem
The icon in the AppHeader top-left is displayed inside a `w-7 h-7 rounded-lg bg-primary` container. The icon image (`/icon-192.png`) does not fill the full container — it sits small on a blue background, leaving visible padding.

## Goal
Make the icon fill the entire container with no visible blue background.

## Approach
Two options to present:

1. **Remove background, scale the image** — Remove `bg-primary` from the container and apply a slight CSS scale (e.g. `scale-110` or `scale-125`) on the `<img>` to compensate for any built-in transparent padding in the PNG. Keep `rounded-lg` on the container for consistent shape.

2. **Make the icon itself the container** — Remove the wrapper `<div>` entirely and render the `<img>` directly as the logo element at the desired size, applying `rounded-lg` to the image itself.

## Preferred approach
Option 1: keep the wrapper for layout stability but remove `bg-primary`, use `scale-110` to crop edge padding, and ensure `object-cover` fills the frame. This is minimal and preserves the existing DOM structure.

## Files to touch
- `src/components/AppHeader.tsx` — lines 168-170 (icon container and `<img>`)