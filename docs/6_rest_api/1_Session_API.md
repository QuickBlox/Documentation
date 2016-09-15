<div class="app-desc">A set of REST API requests to work with QuickBlox session.</div>
<br>

<section id="api-Default" >
    <span id='Create_session' class='on_page_navigation'></span>
    <h1 class="rest_api_summary rest_api_summary_POST"> Create session </h1>
    <div id="api-Default-session1" class="rest_api_rectangle rest_api_rectangle_POST">
      <br>
      <p class="rest_api_title">Description</p>
      <p class="rest_api_description">To receive the session token you have to authenticate your application by requesting url with obligatory parameters. </p>
      <br>
      <p class="rest_api_title">Endpoint</p>
      <div class="rest_api_endpoint rest_api_endpoint_POST">
        <p class="rest_api_description">POST /session.json</p>
      </div>
      <br>
      <!--  -->
        <br>
        <p class="rest_api_title">Parameters</p>
        <table class="rest_api_params_table">
          <thead>
          <tr>
            <th>Name</th>
            <th>Required</th>
            <th>Type</th>
            <th>Value example</th>
            <th>Description</th>
          </tr>
          </thead>
          <tbody>
          <tr>
            <td>applicationId</td>
            <td class="rest_api_params_table_param_value">yes</td>
            <td class="rest_api_params_table_param_value">Integer</td>
            <td class="rest_api_params_table_param_value">92</td>
            <td class="rest_api_params_table_param_value">API Application Identifier</td>
          </tr>
          <tr>
            <td>authKey</td>
            <td class="rest_api_params_table_param_value">yes</td>
            <td class="rest_api_params_table_param_value">String</td>
            <td class="rest_api_params_table_param_value">WzRAY9vrGmb6FfP</td>
            <td class="rest_api_params_table_param_value">Authentication Key.</td>
          </tr>
          <tr>
            <td>timestamp</td>
            <td class="rest_api_params_table_param_value">yes</td>
            <td class="rest_api_params_table_param_value">Integer</td>
            <td class="rest_api_params_table_param_value">1473075233</td>
            <td class="rest_api_params_table_param_value">Unix Timestamp It shouldn&#39;t be differ from time provided by NTP more than 10 minutes. We suggest you synchronize time on your devices with NTP service.</td>
          </tr>
          <tr>
            <td>nonce</td>
            <td class="rest_api_params_table_param_value">yes</td>
            <td class="rest_api_params_table_param_value">Integer</td>
            <td class="rest_api_params_table_param_value">189463</td>
            <td class="rest_api_params_table_param_value">Unique Random Value. Requests with the same timestamp and same value for nonce parameter can not be send twice.</td>
          </tr>
          <tr>
            <td>signature</td>
            <td class="rest_api_params_table_param_value">yes</td>
            <td class="rest_api_params_table_param_value">String</td>
            <td class="rest_api_params_table_param_value">eb0ec2d8c8184a3e62b41
da2afb6e8d690577fa4
</td>
            <td class="rest_api_params_table_param_value">Follow Signature generation guide</td>
          </tr>
          </tbody>
        </table>
      <br>
      <!-- <p class="rest_api_title">Request</p> -->
      <p class="rest_api_title">Response</p>
          <p class="rest_api_description"><strong>Status:</strong> 201</p>
          <p class="rest_api_description"><strong>Content-Type:</strong> application/json</p>
          <p class="rest_api_description"><strong>Return type:</strong> Session object</p>
        <pre class="language-javascript"><code class="language-javascript">{
  "updated_at" : "2012-04-03T07:34:48Z",
  "user_id" : 302,
  "created_at" : "2012-04-03T07:34:48Z",
  "_id" : "0e7bc95d85c0eb2bf052be3d29d3df523081e87f",
  "id" : 326,
  "application_id" : 92,
  "nonce" : 18945,
  "token" : "1e7bc99d85c0eb2bf062be3d29d3df523081e874",
  "ts" : 1473075233
}</code></pre>
    </div>
    <br><br>
    <span id='Retrieve_session_Info' class='on_page_navigation'></span>
    <h1 class="rest_api_summary rest_api_summary_GET"> Retrieve session Info </h1>
    <div id="api-Default-session2" class="rest_api_rectangle rest_api_rectangle_GET">
      <br>
      <p class="rest_api_title">Description</p>
      <p class="rest_api_description">Get info about current session</p>
      <br>
      <p class="rest_api_title">Endpoint</p>
      <div class="rest_api_endpoint rest_api_endpoint_GET">
        <p class="rest_api_description">GET /session.json</p>
      </div>
      <br>
      <!--  -->
      <br>
      <!-- <p class="rest_api_title">Request</p> -->
      <p class="rest_api_title">Response</p>
          <p class="rest_api_description"><strong>Status:</strong> 200</p>
          <p class="rest_api_description"><strong>Content-Type:</strong> application/json</p>
          <p class="rest_api_description"><strong>Return type:</strong> Session object</p>
        <pre class="language-javascript"><code class="language-javascript">{
  "updated_at" : "2012-04-03T07:34:48Z",
  "user_id" : 302,
  "created_at" : "2012-04-03T07:34:48Z",
  "_id" : "0e7bc95d85c0eb2bf052be3d29d3df523081e87f",
  "id" : 326,
  "application_id" : 92,
  "nonce" : 18945,
  "token" : "1e7bc99d85c0eb2bf062be3d29d3df523081e874",
  "ts" : 1473075233
}</code></pre>
    </div>
    <br><br>
    <span id='Destroy_session_' class='on_page_navigation'></span>
    <h1 class="rest_api_summary rest_api_summary_DELETE"> Destroy session  </h1>
    <div id="api-Default-session3" class="rest_api_rectangle rest_api_rectangle_DELETE">
      <br>
      <p class="rest_api_title">Description</p>
      <p class="rest_api_description">Destroy user session (Session token will be decreased to the token of the application) </p>
      <br>
      <p class="rest_api_title">Endpoint</p>
      <div class="rest_api_endpoint rest_api_endpoint_DELETE">
        <p class="rest_api_description">DELETE /session.json</p>
      </div>
      <br>
      <!--  -->
      <br>
      <!-- <p class="rest_api_title">Request</p> -->
      <p class="rest_api_title">Response</p>
          <p class="rest_api_description"><strong>Status:</strong> 200</p>
          <p class="rest_api_description"><strong>Content-Type:</strong> application/json</p>
          <p class="rest_api_description"><strong>Return type:</strong> Empty body</p>
        <pre class="language-javascript"><code class="language-javascript">{ }</code></pre>
    </div>
    <br><br>
</section>

