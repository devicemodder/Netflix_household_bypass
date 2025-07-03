# Netflix_household_bypass
A workaround for the netflix account household error
for desktop is below, but might work on firefox mobile too. 


Install ublock origin in firefox, add this filter: 

    netflix.com##.nf-modal.interstitial-full-screen

**this bypasses the household error overlay.** Yes, it's only an overlay.

but, the video controls are still disabled. Can't use netflix's own controls, so have to use the browser's default controls. 

to do this, you need tamper monkey and add this script:

    // ==UserScript==
    // @name         Netflix Native Controls + Prev/Next/Home Buttons (Top-Left Home)
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

    // === Extract Episode ID from URL ===
    const match = window.location.href.match(/\/watch\/(\d+)/);
    const currentID = match ? parseInt(match[1], 10) : null;
    if (!currentID) return;

    // === Button Factory ===
    const makeButton = (label, onClick, position) => {
        const btn = document.createElement('button');
        btn.textContent = label;
        Object.assign(btn.style, {
            position: 'fixed',
            zIndex: 10000,
            padding: '10px 16px',
            fontSize: '16px',
            background: '#e50914',
            color: 'white',
            border: 'none',
            borderRadius: '4px',
            cursor: 'pointer',
            boxShadow: '0 2px 8px rgba(0,0,0,0.3)',
            userSelect: 'none',
        });

        if (position.side) {
            btn.style[position.side] = position.offset;
            btn.style[position.axis] = position.distance;
        }

        btn.addEventListener('click', onClick);
        document.body.appendChild(btn);
    };

    // ⬅️ Previous Episode (bottom-left)
    makeButton('⬅️ Previous Episode', () => {
        const newID = currentID - 1;
        window.location.href = `https://www.netflix.com/watch/${newID}`;
    }, { side: 'left', axis: 'bottom', offset: '20px', distance: '80px' });

    // ➡️ Next Episode (bottom-right)
    makeButton('Next Episode ➡️', () => {
        const newID = currentID + 1;
        window.location.href = `https://www.netflix.com/watch/${newID}`;
    }, { side: 'right', axis: 'bottom', offset: '20px', distance: '80px' });

    // 🏠 Home (top-left)
    makeButton('🏠 Home', () => {
        window.location.href = 'https://www.netflix.com/browse';
    }, { side: 'left', axis: 'top', offset: '20px', distance: '20px' });

    })();


it'll run and videos will play, but you won't be able to use the full netflix interface. 

Adds Previous/Next episode buttons to the video player (may not work correctly depending on the video. still working the bugs out)

it's a bit clunky but it works.

*In the event that the video does NOT play (shows a still frame) the URL needs to be modified to remove all the extra stuff so it only shows as follows "https://www.netflix.com/watch/<insert video ID number here>"

****In  regards to "will this make netflix ban me?" NO it should not as this is all done cliient side, in browser and as such, shouldn't be detectable.****
