# Netflix_household_bypass
A workaround for the netflix account household error
for desktop is below, but might work on firefox mobile too. 


Install ublock origin in firefox, add this filter: 

    netflix.com##.nf-modal.interstitial-full-screen

**this bypasses the household error overlay.** Yes, it's only an overlay.

but, the video controls are still disabled. Can't use netflix's own controls, so have to use the browser's default controls. 

to do this, you need tamper monkey and add this script:

    // ==UserScript==
    // @name         Netflix Native Controls + Prev/Next Episode Buttons (Split)
    // @match        https://www.netflix.com/watch/*
    // @grant        none
    // ==/UserScript==

    (function () {
    'use strict';

    // === Native Video Controls ===
    const enableControls = () => {
        const video = document.querySelector('video');
        if (video && !video.controls) {
            video.controls = true;
            video.play().catch(() => {});
        }
    };
    enableControls();
    setInterval(enableControls, 2000);

    // === Extract Current Episode ID from URL ===
    const match = window.location.href.match(/\/watch\/(\d+)/);
    const currentID = match ? parseInt(match[1], 10) : null;
    if (!currentID) return;

    // === Button Factory ===
    const makeButton = (label, offset, side, bottomPos) => {
        const btn = document.createElement('button');
        btn.textContent = label;
        btn.style.position = 'fixed';
        btn.style[side] = '20px';
        btn.style.bottom = `${bottomPos}px`;
        btn.style.zIndex = 10000;
        btn.style.padding = '10px 16px';
        btn.style.fontSize = '16px';
        btn.style.background = '#e50914';
        btn.style.color = 'white';
        btn.style.border = 'none';
        btn.style.borderRadius = '4px';
        btn.style.cursor = 'pointer';
        btn.style.boxShadow = '0 2px 8px rgba(0,0,0,0.3)';
        btn.style.userSelect = 'none';
        btn.title = `Go to episode ID ${currentID + offset}`;

        btn.addEventListener('click', () => {
            const newID = currentID + offset;
            const newURL = `https://www.netflix.com/watch/${newID}`;
            console.log(`[Netflix Skip] Going to: ${newURL}`);
            window.location.href = newURL;
        });

        document.body.appendChild(btn);
    };

    // ⬅️ Previous (bottom-left), ➡️ Next (bottom-right)
    makeButton('⬅️ Previous Episode', -1, 'left', 80);
    makeButton('Next Episode ➡️', 1, 'right', 80);
    })();


it'll run and videos will play, but you won't be able to use the full netflix interface. 

Adds Previous/Next episode buttons to the video player (may not work correctly depending on the video. still working the bugs out)

it's a bit clunky but it works.

****In  regards to "will this make netflix ban me?" NO it should not as this is all done cliient side, in browser and as such, shouldn't be detectable.****
