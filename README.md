# Netflix_household_bypass
A workaround for the netflix account household error
for desktop is below, but might work on firefox mobile too. 


Install ublock origin in firefox, add this filter: 

    netflix.com##.nf-modal.interstitial-full-screen

**this bypasses the household error overlay.** Yes, it's only an overlay.

but, the video controls are still disabled. Can't use netflix's own controls, so have to use the browser's default controls. 

to do this, you need tamper monkey and add this script:

    // ==UserScript==
    // @name         Netflix Enable Native Controls
    // @match        https://www.netflix.com/watch/*
    // @grant        none
    // ==/UserScript==

    (function() {
    'use strict';

    const enableControls = () => {
        const video = document.querySelector('video');
        if (video && !video.controls) {
            video.controls = true;
            // Optional: try to play automatically (may require user gesture)
            video.play().catch(() => {});
            console.log("[Netflix Controls] Native controls enabled");
        }
    };

    // Run immediately
    enableControls();

    // Also run periodically in case video element changes dynamically
    setInterval(enableControls, 2000);
    })();

it'll run and videos will play, but you won't be able to use the full netflix interface. 

Also, you have to manually go back to the main page to select the next episode

it's a bit clunky but it works.
