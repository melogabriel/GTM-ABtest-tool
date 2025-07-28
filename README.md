# GTM A/B Test Generator

A/B Testing with GTM Made Easy. Automate your A/B test setup. Generate a ready-to-import GTM container in seconds, not hours.

A simple [web-based tool](https://melogabriel.github.io/GTM-ABtest-tool/) to instantly generate a ready-to-import Google Tag Manager container (`.json`) for a classic A/B test.


---

## Overview

Setting up a simple A/B test (e.g., hiding an element for 50% of users) in Google Tag Manager can be a repetitive, multi-step process. This tool automates the entire setup by generating a container file with all the necessary tags, triggers, and variables.

Simply fill out a form with your test parameters, download the `.json` file, and import it into your GTM container.

## Features

* **Simple Interface:** No-nonsense form to define your test parameters.
* **Instant JSON Generation:** Creates a valid GTM container file on the fly.
* **Customizable:** Define your own cookie names, element selectors, and GA4 parameters.
* **Standard Implementation:** Uses a robust, standard method for A/B testing with a 1st-party cookie.
* **GA4 Integration:** Automatically creates a GA4 event tag to send the A/B test group information to Google Analytics.

## How It Works

The tool generates a GTM container with three core components:

1.  **Custom HTML Tag:** This script runs on page load. It checks if a user has already been assigned to an A/B test group by looking for a specific cookie.
    * If no cookie is found, it randomly assigns the user to "Group A" or "Group B" and sets a 1st-party cookie to remember that assignment for a specified number of days.
    * If the user is in "Group B", the script hides the HTML element you specified.
2.  **1st-Party Cookie Variable:** This variable reads the value ("A" or "B") from the cookie set by the Custom HTML tag.
3.  **GA4 Event Tag:** This tag fires on page load and sends a `page_view` event to Google Analytics. Crucially, it attaches the value from the cookie variable as an event parameter, allowing you to analyze your test results in GA4.

All these components are set to fire on a single "All Pages" trigger.

## How to Use

1.  **Navigate to the Tool:** [**GTM A/B Test Generator**](https://melogabriel.github.io/GTM-ABtest-tool/)
2.  **Fill in the Fields:**
    * **Test Name:** A descriptive name for your test (e.g., "Homepage Hero Button Test"). This will be used to name the components in GTM.
    * **Cookie Name:** The name of the cookie that will store the user's group (e.g., `ab_test_hero_button`).
    * **Element Selector to Hide:** The CSS selector for the element you want to hide for Group B (e.g., `#hero-button`, `.pricing-table`).
    * **GA4 Parameter Name:** The name of the parameter that will be sent to GA4 (e.g., `hero_button_variant`). **This is important for your GA4 setup.**
    * **GA4 Measurement ID:** Your GA4 Measurement ID (e.g., `G-XXXXXXXXXX`).
    * **Cookie Duration:** How long (in days) a user should remain in their assigned group.
3.  **Download the JSON:** Click the "Download GTM Container JSON" button.
4.  **Import into GTM:**
    * In your GTM container, go to **Admin → Import Container**.
    * Select the `.json` file you just downloaded.
    * Choose the workspace you want to add it to.
    * **IMPORTANT:** Select the **MERGE** import option. You will be asked to choose whether to **Overwrite** or **Rename** conflicting tags. It is safest to choose **Rename**.
    * Review the changes. You will see the new tags, trigger, and variable that will be added.
    * Click **Confirm**.

## Crucial Next Step: Google Analytics 4 Setup

To see your A/B test data in GA4 reports, you must register the parameter you created as a **Custom Dimension**.

1.  In your Google Analytics 4 property, go to **Admin → Custom definitions**.
2.  Under the "Custom dimensions" tab, click **Create custom dimensions**.
3.  Configure the dimension:
    * **Dimension name:** A user-friendly name (e.g., "A/B Hero Button Variant").
    * **Scope:** **Event**
    * **Event parameter:** This must **exactly match** the "GA4 Parameter Name" you entered in the tool (e.g., `hero_button_variant`).
4.  Click **Save**.

It can take up to 24-48 hours for data to appear in your GA4 reports for this new custom dimension. Once it does, you can use it to compare the behavior of Group A vs. Group B.

## Contributing

Feedback and contributions are welcome! If you find a bug or have an idea for an improvement, please open an issue or submit a pull request.

## License

This project is licensed under the [MIT License](LICENSE).
