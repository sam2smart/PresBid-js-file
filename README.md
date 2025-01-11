# PresBid-js-file

Requirement	Implemented
Prebid.js Setup	== Configured with AppNexus and Rubicon.
Responsive Ad Units == Sizes defined for desktop and mobile.
Bid Adapter Integration	== Two adapters integrated and tested.
Floor Price Implementation == Dynamic pricing based on size and device.
Analytics Integration == Google Analytics integrated.
Error Handling	==  Logs and fallback ads implemented.
Fallback Ads == Custom fallback ads displayed on failure.
Bid Validation == OpenRTB-compliant validation via Prebid.js.
Lazy Loading Ads  ==  Intersection Observer for lazy loading.
CI/CD Pipeline Integration  ==  GitHub Actions for automated deployment.


* Setup Process and Tools Used

 1. Install and Include Prebid.js
Prebid.js is the core library for implementing header bidding. Include it in the HTML file:
<script src="https://cdn.jsdelivr.net/npm/prebid.js@latest"></script>

 2. Configure Ad Units
Define ad units in prebid-config.js with ad slot IDs, sizes, and SSP configurations.
Example:
const adUnits = [
  {
    code: 'ad-slot-1',
    mediaTypes: { banner: { sizes: [[300, 250], [320, 50]] } },
    bids: [
      { bidder: 'appnexus', params: { placementId: '12345' } },
      { bidder: 'rubicon', params: { accountId: '67890', zoneId: '24680' } }
    ]
  }
];

 3. Implement Floor Pricing
Set dynamic floor prices for better optimization.
Example:
adUnits.forEach(adUnit => {
  adUnit.floors = {
    default: 0.50,
    rules: [
      { mediaType: 'banner', size: [300, 250], floor: 1.00 },
      { mediaType: 'banner', size: [320, 50], floor: 0.75 }
    ]
  };
});

 4. Add Bid Adapters (SSPs)
Integrate multiple SSPs by adding bid adapters (e.g., AppNexus, Rubicon).

 5. Handle Bidding and Responses
Request bids with Prebid.js and set targeting for Google Publisher Tags (GPT).
Example:
pbjs.requestBids({
  bidsBackHandler: function (bids) {
    pbjs.setTargetingForGPTAsync();
    googletag.pubads().refresh();
  }
});

 6. Analytics Integration
Track bid performance, win rates, and latency using analytics (e.g., Google Analytics).
Example:
pbjs.onEvent('bidWon', function (data) {
  gtag('event', 'Bid Won', { event_category: 'Ads', event_label: data.adUnitCode, value: data.cpm });
});

 7. Error Handling
Log errors and implement fallback ads if no valid bids are received.
Example:
pbjs.onEvent('auctionEnd', function (data) {
  if (!data) { loadFallbackAds(); }
});

 8. Lazy Load Ads
Use the Intersection Observer API to load ads only when they come into view.
Example:
const observer = new IntersectionObserver(lazyLoad, { threshold: 0.5 });
ads.forEach(ad => observer.observe(ad));

 9. CI/CD Pipeline
Automate deployment with GitHub Actions or GitLab CI/CD.
Example (GitHub Actions):
name: CI/CD Deployment
on: push
jobs:
  deploy:
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3
      - name: Install Dependencies
        run: npm install
      - name: Deploy
        run: npm run deploy

 10. Final Deployment
Deploy the site with Prebid.js managing bidding from SSPs, applying floor prices, and ensuring optimal ad rendering.

* Tools Used
- Prebid.js: Core library for header bidding.
- Google Publisher Tags (GPT): Ad rendering and targeting.
- Google Analytics: Analytics tracking for bids and performance.
- Intersection Observer API: Lazy loading ads for performance.
- GitHub Actions: CI/CD pipeline for automated deployment.
