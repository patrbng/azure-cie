# Exercise - Secure access to resources

_Recording of exercise review_
_https://siriusazureciestg.blob.core.windows.net/videos/section1/7-Exercise-Recording.mp4_

<!--- We need to add steps to starting the AD Premium trial and assigning licenses followed by a signout/signin --->

* 3 minutes

In the previous exercise, we enabled trial licenses, created a directory, created a user, and created a group to test our solution. In this unit, we will create our conditional access rule to require Azure Multi-Factor Authentication for the Azure portal.

## Login to the Azure Portal

1. Open the [Azure portal](https://portal.azure.com) in a browser.

2. Sign into Azure using the Microsoft account email address and password you created for this session.

## Enable conditional access-based Multi-Factor Authentication

Conditional access allows administrators to configure when they do or do not want something to happen. They can use multiple rules in parallel to grant or deny access to resources. Here's the rule that we need to create:

### When accessing the Azure portal - Require multi-factor authentication

The steps that follow will walk you through the process to create a conditional access rule to require users to perform multi-factor authentication when they access the Azure portal.

1. Browse to **Azure Active Directory**, and in the resulting blade locate and select **Conditional access** in the **Security** subsection.

    ![A screenshot showing Azure Portal navigation items with selection boxes drawing attention to the Azure Active Directory item and the Conditional access item](images/securedirectory1.png)

2. Click **New policy**. This will open a new blade for creating the new policy.

3. Name the policy **Require MFA for Azure portal**.

4. Under **Assignments** select **Users and groups** to open the next blade.

5. Click **Select users and groups** and then **Select** below it. This will open the selection blade.

6. Select the group that we created in the previous unit, **CA-MFA-AzurePortal**, then click the **Select** button at the bottom. This will close the selection blade.

7. Click the **Done** button to complete this step and close the blade. You should once again see the new policy blade you opened in the second step above.

8. Under **Assignments** select **Cloud apps** to open the next blade.

9. Click **Select apps** and then **Select** below it. This will open the selection blade.

10. Select **Microsoft Azure Management**, and then the **Select** button at the bottom. This will close the selection blade.

11. Click the **Done** button to complete this step and close the blade. You should once again see the new policy blade you opened in the second step above.

12. Under **Access controls** select **Grant** to open the next blade.

13. Click **Grant access** and then **Require multi-factor authentication**. Click the **Select** button to complete this step and close the blade. You should once again see the new policy blade you opened in the second step above.

14. Set **Enable policy** to **On**.

15. Click **Create**. If the **Create** button is not enabled, review the previous steps to ensure you've completed them all correctly.

In this unit, you learned how to create a conditional access rule for a previously defined group and/or user(s). The rule requires Multi-Factor Authentication when accessing the Azure portal.
