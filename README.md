REPORT z_display_name.

DATA: lv_name TYPE string.

lv_name = 'Aju Abraham'.

WRITE: / 'The name is:', lv_name.
***********************************************
REPORT z_rest_api_example.

DATA: lo_http_client TYPE REF TO if_http_client,
      lv_url         TYPE string,
      lv_response    TYPE string,
      lv_status_code TYPE i.

lv_url = 'https://jsonplaceholder.typicode.com/posts/1'. " Example REST API URL

TRY.
    " Create an HTTP client
    cl_http_client=>create_by_url(
      EXPORTING
        url                = lv_url
      IMPORTING
        client             = lo_http_client
    ).

    " Set the request method (GET)
    lo_http_client->request->set_method( 'GET' ).

    " Send the HTTP request
    lo_http_client->send( ).

    " Get the response status code
    lv_status_code = lo_http_client->response->get_status( ).

    " Get the response body
    lv_response = lo_http_client->response->get_cdata( ).

    " Display the response status and body
    WRITE: / 'Status Code: ', lv_status_code.
    WRITE: / 'Response: ', lv_response.

  CATCH cx_http_client_error INTO DATA(lx_http_error).
  ***************************************************************************************************************
  REPORT z_restful_sap_ai_example.

DATA: lo_http_client TYPE REF TO if_http_client,
      lv_url         TYPE string,
      lv_token       TYPE string,  " Authorization token (e.g., OAuth token)
      lv_request     TYPE string,  " JSON request payload
      lv_response    TYPE string,  " Response from SAP AI
      lv_status_code TYPE i,       " Status code of the response
      lv_error_msg   TYPE string.  " Error message in case of failure

lv_url = 'https://<SAP_AI_SERVICE_URL>/predict'.  " Replace with actual SAP AI API endpoint
lv_token = 'Bearer <your_access_token>'.  " Replace with your actual Bearer token for authentication

" Create the request body (example JSON for SAP AI document processing)
lv_request = '{ "document": "sample_document.pdf" }'.  " Adjust based on the API you're calling

TRY.
    " Create an HTTP client
    cl_http_client=>create_by_url(
      EXPORTING
        url                = lv_url
      IMPORTING
        client             = lo_http_client
    ).

    " Set Authorization header (e.g., OAuth Bearer Token)
    lo_http_client->request->set_header_field( name = 'Authorization' value = lv_token ).

    " Set Content-Type and Accept headers for JSON
    lo_http_client->request->set_header_field( name = 'Content-Type' value = 'application/json' ).
    lo_http_client->request->set_header_field( name = 'Accept' value = 'application/json' ).

    " Set the HTTP method (POST)
    lo_http_client->request->set_method( 'POST' ).

    " Set the request body (JSON data)
    lo_http_client->request->set_cdata( lv_request ).

    " Send the HTTP request
    lo_http_client->send( ).

    " Get the response status code
    lv_status_code = lo_http_client->response->get_status( ).

    " Get the response body (the AI response)
    lv_response = lo_http_client->response->get_cdata( ).

    " Handle the response status code
    IF lv_status_code = 200.
        WRITE: / 'AI Service Response:', lv_response.
    ELSE.
        WRITE: / 'Error: ', lv_status_code, lv_response.
    ENDIF.

  CATCH cx_http_client_error INTO DATA(lx_http_error).
    lv_error_msg = lx_http_error->get_text( ).
    WRITE: / 'HTTP Client Error: ', lv_error_msg.
ENDTRY.
*********************************************************************************************************************************************
    WRITE: / 'Error: ', lx_http_error->get_text( ).
ENDTRY.
