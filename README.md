# Moesif plugin for Google Tag Manager

- [Moesif](https://www.moesif.com/) is an API analytics service. This tag enables you to track user behavior within your web app. You can also install the Moesif server agent to understand the customer journey across both your website and APIs together.

- [Google Tag Manager account](https://tagmanager.google.com/) is a free application which enables marketing and product teams to quickly add script tags for analytics, attribution tracking, and more without code changes. 

> You can use this plugin alongside a [Moesif server integration](https://www.moesif.com/implementation) to monitor server-side API traffic. This enables you to track your end-to-end customer journey and build cross-platform funnel reports like your _initial sign up_ to _first API call_ conversion rate.

![Diagram of Moesif API monitoring and Google Tag Manager architecture](https://www.moesif.com/docs/images/docs/client-integration/moesif-arch-google-tag-manager.png)

## How to install

Moesif has a GTM tag in the [Google Tag Manager template gallery](https://tagmanager.google.com/gallery/#/owners/Moesif/templates/moesif-gtm-plugin).
This installs the [moesif-browser-js](https://www.moesif.com/implementation/track-user-behaviors-with-browser?platform=browser) script without any code changes. 

### 1. Add Tag

Log into your [Google Tag Manager account](https://tagmanager.google.com/) and select Tags from the left menu.
Then, click _New_ from the top right like in the below picture.

![Create a new Tag in Google Tag Manager](https://www.moesif.com/docs/images/docs/client-integration/google-tag-manager-add-tag.png)

Select the _Community Template Gallery_ blue banner and search for "Moesif API Analytics".
Then, click the blue _Add to Workspace_ button.

### 2. Set Moesif Application Id

In the Tag configuration, add your Moesif application Id. 
Your Moesif Application Id will be displayed during the onboarding steps when signing up for [Moesif](https://www.moesif.com/). 

By default, the _Action Name_ is set to "Page View". We recommend setting a descriptive name based on your chosen trigger in next step. 
For example, if the tag is fired on your Sign In Page, then set _Action Name_ to "Sign In"

![Add Moesif in Google Tag Manager](https://www.moesif.com/docs/images/docs/client-integration/google-tag-manager-select-moesif.png)

### 3. Choose a Trigger

Choose a trigger that will cause the tag should be fired such as "All Pages".

![Add Moesif in Google Tag Manager](https://www.moesif.com/docs/images/docs/client-integration/google-tag-manager-select-moesif.png)

For more info on how to create custom triggers, see [Google's documentation.](https://support.google.com/tagmanager/answer/7679316?hl=en)

## How to use

When the tag is fired, the tag will log a [user action](https://www.moesif.com/docs/getting-started/user-actions/) to Moesif.

### Event metadata

The Tag also supports [event metadata](https://www.moesif.com/docs/getting-started/user-actions/#event-metadata) which is a table of key/value pairs. 
The tag also supports dynamic variables. 

![Add Moesif in Google Tag Manager](https://www.moesif.com/docs/images/docs/client-integration/google-tag-manager-add-event-metadata.png)

You can access any of the [moesif-browser-js APIs](https://www.moesif.com/docs/client-integration/browser-js/) via `window.moesif` including [identifyUser()](https://www.moesif.com/docs/client-integration/browser-js/#identifying-users). This can be useful since Google Tag Manager doesn't support tracking user ids directly. 

### Identifying users

When you know the user's id such as after sign in, call `identifyUser`.
This tells Moesif who the current user is so their actions can be linked to a user profile.

Besides the userId, you can also save related properties such as user demographics and subscription information.

```javascript
window.moesif.identifyUser("12345", {
  email: "john@acmeinc.com",
  firstName: "John",
  lastName: "Doe",
  title: "Software Engineer",
  salesInfo: {
    stage: "Customer",
    lifetimeValue: 24000,
    accountOwner: "mary@contoso.com",
  },
});
```

The recommended place to call `identifyUser` is after the user logs in and you know their real user id.

> Do not call `identifyUser` for anonymous users. Moesif will track these users automatically.

### Identifying companies

In addition to identifying users, you can also identify the company for account-level tracking. Besides the companyId, you can also save related properties such as company demographics and website domain.

```javascript
// Only the first argument is a string containing the company id. This is the only required field.
// The second argument is a object used to store a company info like plan, MRR, and company demographics.
// The third argument is a string containing company website or email domain. If set, Moesif will enrich your profiles with publicly available info.
metadata = {
  orgName: "Acme, Inc",
  planName: "Free Plan",
  dealStage: "Lead",
  mrr: 24000,
  demographics: {
    alexaRanking: 500000,
    employeeCount: 47,
  },
};

window.moesif.identifyCompany("67890", metadata, "acmeinc.com");
```

> If you call both identifyUser() and identifyCompany() in the same session, then Moesif will automatically associate the user with the company.

### Tracking actions directly
Since you have the entire [moesif-browser-js API](https://www.moesif.com/docs/client-integration/browser-js/) 
available through `window.moesif`, you can track actions directly using 
the [`track()`](https://www.moesif.com/docs/client-integration/browser-js/#track-user-actions) function:

```javascript
window.moesif.track('clicked_sign_up', {
  button_label: 'Get Started'
});
```