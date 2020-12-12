# Moesif plugin for Google Tag Manager

- [Moesif](https://www.moesif.com/) is an API analytics service. This tag enables you to track user behavior within your web app. You can also install the Moesif server agent to understand the customer journey across both your website and APIs together.

- [Google Tag Manager account](https://tagmanager.google.com/) is a free application which enables marketing and product teams to quickly add script tags for analytics, attribution tracking, and more without code changes. 

## How to install

This plugin is installed within the [Google Tag Manager UI](https://tagmanager.google.com/) and will add [moesif-browser-js](https://www.moesif.com/implementation/track-user-behaviors-with-browser?platform=browser) without any code changes. 

### 1. Add Tag

Log into your [Google Tag Manager account](https://tagmanager.google.com/) and select Tags from the left menu.
Then, click _New_ from the top right like in the below picture and search for "Moesif API Analytics".

![Create a new Tag in Google Tag Manager](https://www.moesif.com/docs/images/docs/client-integration/google-tag-manager-add-tag.png)

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

### Identifying users

You can access any of the [moesif-browser-js APIs](https://www.moesif.com/docs/client-integration/browser-js/) via `window.moesif` including [identifyUser()](https://www.moesif.com/docs/client-integration/browser-js/#identifying-users). This can be useful since Google Tag Manager doesn't support tracking user ids directly. 

When a user logs into your app, identify the person with Moesif like so: 

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
### Event metadata

When the tag is fired, the Moesif track() method will be called with your chosen action name.
The Tag also supports [event metadata](https://www.moesif.com/docs/getting-started/user-actions/#event-metadata) which is a table of key/value pairs. 
The tag also supports dynamic variables. 

![Add Moesif in Google Tag Manager](https://www.moesif.com/docs/images/docs/client-integration/google-tag-manager-add-event-metadata.png)

