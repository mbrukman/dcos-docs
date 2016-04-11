---
UID: 56fb10d937a6a
post_title: Creating ACL Group Definitions
post_excerpt: ""
layout: page
published: true
menu_order: 0
page_options_require_authentication: false
page_options_show_link_unauthenticated: false
hide_from_navigation: true
hide_from_related: true
---
You can define fine-grained access to applications that are running in DCOS by defining advanced ACL groups. Advanced ACL groups can provide multi-tenancy by isolating application teams, and individual users. You can also control customized access to applications, for example read-only access. This access is administered on the backend by Admin Router and the native Marathon instance.

Authorization is not secure, but provides user isolation. Anything running inside the cluster, and anyone SSH’d into it, has access to everything.
    

You can create advanced ACL groups for each role in your organization. 

To create an advanced ACL group:

1.  Launch the DCOS web interface and login with your Admin username and password.

2.  Click on the **System** tab, select **Organization**, and then **Groups**.

3.  Click **New Group** button and provide a name for your group.

4.  Open the new group and click **Advanced ACLs**.

5.  Define your advanced ACL by providing Resource and Permission and then click **Add Rule**.

    ![alt text](/assets/images/auth-enable-advanced-acl.gif)
    
    Here are the available resource options.
    
    ### DCOS core components
    Access to these resources is controlled by the Admin Router.
    
    <p>
    <table class="table">
      <tr>
        <th>Resource</th>
        <th>Permission</th>
      </tr>
      <tr>
        <td><code>dcos:adminrouter:package</code><p>This resource controls access to the DCOS packaging API. Users with access can install DCOS services from the DCOS package repository.</p></td>
        <td><code>full</code></td>
      </tr>
      <tr>
        <td><code>dcos:adminrouter:ops:mesos</code><p>This resource controls access to Mesos UI. Users with access can navigate to the Mesos UI.</p></td>
        <td><code>full</code></td>
      </tr>
      <tr>
        <td><code>dcos:adminrouter:ops:exhibitor</code><p>This resource controls access to Exhibitor UI.</p></td>
        <td><code>full</code></td>
      </tr>
      <tr>
        <td><code>dcos:adminrouter:ops:slave</code><p>This resource controls access to Admin Router's agent endpoint.</p></td>
        <td><code>full</code></td>
      </tr>
      <tr>
        <td><code>dcos:adminrouter:service:marathon</code><p>This resource controls access to the DCOS Marathon instance. For example, you can grant a user access to `http://<public-master-IP>/service/marathon`.</p></td>
        <td><code>full</code></td>
      </tr>
      <tr>
        <td><code>dcos:adminrouter:service:&lt;service-name&gt;</code><p>This resource controls access to installed DCOS Services.</p></td>
        <td><code>full</code></td>
      </tr>
      <tr>
      <td><code>dcos:superuser</code><p>This resource controls access to the the superuser group. This group has access to all DCOS components and is where DCOS administrator access is defined.</p></td>
      <td><code>full</code></td>
    </tr>
      <tr>
      </tr>
    </table>
    </p>
    <!-- For 1.9 *   `dcos:acs` - resources defined by the access control service. -->

    ### DCOS Marathon 
    Access to these resources is controlled by the DCOS Marathon instance.
    <p>
    <table class="table">
      <tr>
        <th>Resource</th>
        <th>Permission</th>
      </tr>
              <tr>
              <td><code>dcos:service:marathon:marathon:admin</code><p>This resource control access to specific operator endpoints. <!-- What are these endpoints? --></p></td>
              <td><code>full</code></td>
            </tr>
      <tr>
        <td><code>dcos:service:marathon:marathon:services</code><p>This resource controls access to all apps managed by DCOS Marathon.</p></td>
        <td><code>create</code>, <code>read</code>,<code>update</code>, <code>delete</code></td>
      </tr>         
      <tr>
        <td><code>dcos:service:marathon:marathon:services/&lt;service-group&gt;</code><p>This resource controls group access to the DCOS Marathon instance.</p></td>
        <td><code>create</code>, <code>read</code>,<code>update</code>, <code>delete</code></td>
      </tr>
      <tr>
        <td><code>dcos:service:marathon:&lt;user-marathon&gt;:services/&lt;service-group&gt;</code><p>This resource controls group access to an installed Marathon instance (`<user-marathon>`).</p></td>
        <td><code>create</code>, <code>read</code>,<code>update</code>, <code>delete</code></td>
      </tr>
    </table>
    </p>
            


# Examples 

## Grant user admin access to DCOS
In this example, superuser access is granted to an existing user named Carlos.

1.  Click on the **System** and then the **Organization** tab. 

1.  Click **Groups** and select **Superuser group**.

1.  Click **Members** **Add User** and add Carlos to this group.

    ![add user carlos](/assets/images/auth-enable-ex-superuser.gif)
    
1.  Click **Close** to save these changes.

1.  Send Carlos a link to the web interface.

## Grant user access to a Marathon app group
In this example, a user is granted access to a specific app group in Marathon.

1.  Click on the **System** and then the **Organization** tab.


## Grant user access to install packages
In this example, a user named Ted is granted access to install services from the DCOS package repository.

1.  Click on the **System** and then the **Organization** tab. 

1.  From the **Groups** tab click **New Group**. 

1.  Enter `DCOS package` for Group name and click **Create**.

    ![alt text](/assets/images/auth-enable-ex-package-user.gif)
    
1.  Open the DCOS package group. 

    1.  From the **Advanced ACLs** tab, enter Resource `dcos:adminrouter:package`, Permission `full`, and click **Add Rule**.

        ![alt text](/assets/images/auth-enable-ex-package-user2.gif)
            
    1.  From the **Members** tab, select Ted from the dropdown menu.
    
    1.  Click **Close** to save the changes.
    
1.  Log in to the CLI as Ted.

        $ dcos package install kafka
        52.26.144.2's username: ted
        ted@52.26.144.2's password: 
        [core.dcos_acs_token]: set to 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1aWQiOiJ0ZWQiLCJleHAiOiIxNDYwNDE1NjY0In0.IGk3OOPCpoEUTnaY-V4OMJ1DOGl8YWau8Hd4uedc5lI'
        This will install Apache Kafka DCOS Service.
        Continue installing? [yes/no]


 [1]: https://www.ietf.org/rfc/rfc3986.txt