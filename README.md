# YouTube Bulk Unsubscribe Script

A JavaScript script to automate unsubscribing from multiple YouTube channels.

## How to Use
1. Go to your YouTube Subscriptions page: (https://www.youtube.com/feed/channels).
2. Open your browser's Developer Tools (F12).
3. Navigate to the Console tab, paste the script, and press Enter.

## Script
```javascript
(async () => {
    const delay = ms => new Promise(resolve => setTimeout(resolve, ms));

    async function unsubscribeFromAll() {
        const buttons = Array.from(document.querySelectorAll('button'))
            .filter(button => button.innerText.trim() === 'Subscribed' || button.innerText.trim() === 'Abone olundu');

        console.log(`Number of subscriptions found: ${buttons.length}`);

        for (let i = 0; i < buttons.length; i++) {
            const button = buttons[i];
            button.click();
            console.log(`Menu opened: ${i + 1}/${buttons.length}`);
            await delay(1000);

            const menuItems = Array.from(document.querySelectorAll('tp-yt-paper-item yt-formatted-string'));
            const unsubscribeOption = menuItems.find(item => item.innerText.trim() === 'Unsubscribe' || item.innerText.trim() === 'Abonelikten çık');
            
            if (unsubscribeOption) {
                unsubscribeOption.click();
                console.log(`Unsubscribe option clicked: ${i + 1}/${buttons.length}`);
                await delay(1000);

                const confirmButton = document.querySelector('button[aria-label="Unsubscribe"]') || 
                                      Array.from(document.querySelectorAll('button')).find(btn => btn.innerText.trim() === 'Unsubscribe');
                if (confirmButton) {
                    confirmButton.click();
                    console.log(`Unsubscribed: ${i + 1}/${buttons.length}`);
                    await delay(2000);
                } else {
                    console.log(`Unsubscribe confirmation button not found: ${i + 1}/${buttons.length}`);
                }
            } else {
                console.log(`Unsubscribe option not found in the menu: ${i + 1}/${buttons.length}`);
            }
        }
        console.log('All actions completed.');
    }

    await unsubscribeFromAll();
})();

