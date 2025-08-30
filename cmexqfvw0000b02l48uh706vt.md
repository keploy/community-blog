---
title: "Understanding and Using gRPC Error Codes: A Comprehensive Guide"
seoTitle: "gRPC Error Codes Explained"
seoDescription: "Learn how to effectively navigate and handle gRPC error codes, understand their meanings, and implement best practices for robust error management"
datePublished: Sat Aug 30 2025 03:59:17 GMT+0000 (Coordinated Universal Time)
cuid: cmexqfvw0000b02l48uh706vt
slug: using-grpc-error-codes
canonical: https://keploy.io/blog/community/using-grpc-error-codes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752410917032/34952ec5-4dcc-4acb-9ea2-304b7af13606.png
tags: error, development, apis, grpc

---

Has your gRPC service thrown a `RESOURCE_EXHAUSTED` error right when your boss was explaining to the investors? As we know, one minute everything is working perfectly and the next minute I am frantically trying to decode what feels like a disaster while everyone stares at a blank screen.

Here is what I have learn. gRPC error codes are not just random numbers designed to ruin your day. They are actually incredibly detailed status reports that tell you exactly what went wrong, and if you know how to read them. In this blog, we will know about 17 error codes with the real scenarios and how to debug them.

## What Are gRPC Error Codes Anyway?

![what-are-grpc-code?](https://cdn.hashnode.com/res/hashnode/image/upload/v1753210171727/c8591296-f2b0-4224-8bbd-478908817289.png align="center")

Okay, we will start with the basics first! When you issue a gRPC request, one of two scenarios occurs: either everything goes smoothly and get an OK status, or something goes wrong and you get an error code. These are not just random numbers. They are carefully designed status codes that clearly indicate what went wrong.

Consider gRPC error codes as HTTP status codes, but designed specifically for remote procedure calls. Similar to how HTTP has 404 for ‘‘Not Found” and 500 for “Internal Server Error”. [gRPC](https://keploy.io/blog/community/what-is-grpc) also has its own set of significant error codes, as shown below.

## The Complete List of gRPC Error Codes

Here's the full rundown of all 17 gRPC error codes. I've organized them in a way that makes sense based on how you'll actually encounter them:

### Success Status

* `OK (0)`: It means everything worked perfectly. This is what you want to see! Your gRPC call completed successfully without any issues and the client received the expected response from the server.
    

### Client-Side Errors (You did this)

* `INVALID_ARGUMENT (3)`: You sent bad data. This happens when your request doesn't meet the server's validation requirements - maybe malformed data, missing required fields, or invalid data types.
    
* `NOT_FOUND (5)`: The thing you're looking for doesn't exist. This is the gRPC equivalent of HTTP 404 - you're asking for a specific resource that simply isn't in the system. Your request format is correct, but the target resource (user, file, or order) was never created or has been deleted.
    
* `ALREADY_EXISTS (6)`: You're trying to create something that already exists. This typically occurs during CREATE operations when you attempt to create a resource with an identifier that's already taken.
    
* `PERMISSION_DENIED (7)`: You don't have permission to do what you're trying to do. Your authentication is valid (the server knows who you are), but you lack the necessary authorisation to perform this specific action. This is about roles and permissions - like a regular user trying to access admin-only features.
    
* `UNAUTHENTICATED (16)`: Your credentials are missing or invalid. This happens when the server can't verify who you are because your authentication token is missing, expired, malformed, or incorrect. Unlike PERMISSION\_DENIED, this isn't about what you're allowed to do - it's about proving your identity first.
    
* `FAILED_PRECONDITION (9)`: The system isn't in the right state for your operation. Your request is valid, but the current system state prevents it from being executed. For example, trying to withdraw money when an account is frozen, or deleting a user who still has active subscriptions.
    
* `OUT_OF_RANGE (11)`: You're trying to access something outside the valid range. This occurs when you request data beyond acceptable boundaries, like requesting page 1000 when only 50 pages exist. It's about exceeding limits and boundaries that the system has defined.
    

### Server-Side Errors (Not my Fault errors)

* `INTERNAL (13)`: In this, something broke on the server side. This is the server equivalent of "it's not you, it's me" - an unexpected error in the server's internal logic. It could be a database connection failure, a required service being down, or a bug in the server code.
    
* `UNIMPLEMENTED (12)`: The method you're calling doesn't exist or isn't supported. The server doesn't know how to handle the specific RPC method you're trying to call. This could mean the method hasn't been implemented yet, was deprecated and removed, or you're calling the wrong method name.
    
* `UNAVAILABLE (14)`: The service is down or overloaded. The server is temporarily unable to handle your request, usually due to maintenance, system overload, or infrastructure issues. This is different from INTERNAL because the server is functioning but can't accept new requests right now.
    
* `RESOURCE_EXHAUSTED (8)`: The server ran out of resources. The server has hit some kind of limit - memory, disk space, database connections, API quotas, or rate limits. Your request might be fine, but the server can't handle it because it's already at capacity.
    
* `DATA_LOSS (15)`: Some data got corrupted or lost. This is a serious error indicating that data has been permanently lost or corrupted due to hardware failures, database corruption, or data migration issues. This is rare but critical - immediate attention is usually required when this error occurs.
    

### Network and Timing Issues

* `DEADLINE_EXCEEDED (4)`: Your call took too long and timed out. The server didn't respond within the configured timeout period, so the client gave up waiting. This doesn't necessarily mean the server is broken - it might be processing a complex request or dealing with slow downstream services.
    
* `CANCELLED (1)`: The operation was cancelled, usually by the client. Someone actively decided to stop the operation before it completed - like a user cancelling a long-running operation or the client application shutting down.
    
* `UNKNOWN (2)`: Something went wrong, but we're not sure what. This is the catch-all error for situations that don't fit into other categories. It might occur when the server encounters an unexpected condition or when there are issues mapping errors between different systems.
    
* `ABORTED (10)`: The operation was aborted, often due to concurrency issues. The server started processing your request but had to stop, usually due to conflicts with other operations.
    

## Real-World Examples: When You'll See These Errors

Let me give you some examples of when you might encounter these errors:

### DEADLINE\_EXCEEDED

This is probably the most common one you'll see. Let's say you're calling a service that processes large files:

```go
// Your call times out after 5 seconds
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()

response, err := client.ProcessLargeFile(ctx, request)
if err != nil {
    // You might get DEADLINE_EXCEEDED if the file is too big
}
```

### UNAVAILABLE

This usually happens when the service is down or during deployments:

```python
try:
    response = client.GetUserProfile(request)
except grpc.RpcError as e:
    if e.code() == grpc.StatusCode.UNAVAILABLE:
        # Service is down, maybe retry with backoff
        time.sleep(1)
        # retry logic here
```

### INVALID\_ARGUMENT

This is your typical validation error:

```java
// If you send an empty user ID
UserRequest request = UserRequest.newBuilder()
    .setUserId("") // Empty string
    .build();

// Server returns INVALID_ARGUMENT
```

## The Two Error Models You Should Know About

Here's something that caught me off guard initially – gRPC has two different error models:

### 1\. Standard Error Model

This is the basic one that every gRPC implementation supports. You get a status code and an optional error message. Simple and universal.

### 2\. Richer Error Model

This is the fancy one that Google uses internally. It lets you include additional structured error details using Protocol Buffers. It's supported in C++, Go, Java, Python, and Ruby, but not everywhere.

The richer model is great when you need to provide detailed error information, but stick with the standard model unless you have a specific need for the extra complexity.

## Best Practices

1. **Always handle errors gracefully** – don't just crash
    
2. **Use appropriate retry logic** – retry transient errors, not permanent ones
    
3. **Log both code and message** – you need both for debugging
    
4. **Choose the right error code** – be specific when implementing servers
    
5. **Monitor error rates** – they're great indicators of system health
    
6. **Test error scenarios** – don't just test the happy path
    

## Debugging gRPC Errors: Tools and Techniques

![Debugging-gRPC-Errors](https://cdn.hashnode.com/res/hashnode/image/upload/v1753210400334/78fb16cb-2ae5-439f-8ef1-b5a5e9d827e3.png align="center")

When you're dealing with gRPC errors in production, having the right debugging tools can save you hours of frustration. Here are my go-to techniques:

### Use gRPC Reflection for Service Discovery

When you get an `UNIMPLEMENTED` error, sometimes it's because you're calling the wrong method name. gRPC reflection lets you discover available methods:

```bash
# Install grpcurl first
go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest

# List all services
grpcurl -plaintext localhost:8080 list

# List methods for a service
grpcurl -plaintext localhost:8080 list myservice.UserService
```

### Enable Detailed Error Logging

Most gRPC implementations let you enable detailed logging to see exactly what's happening:

```go
// Go - Enable verbose logging
os.Setenv("GRPC_GO_LOG_VERBOSITY_LEVEL", "99")
os.Setenv("GRPC_GO_LOG_SEVERITY_LEVEL", "info")
```

### Use Interceptors for Centralized Error Handling

Instead of handling errors in every single method, use interceptors:

```go
func errorInterceptor(ctx context.Context, req interface{}, info *grpc.UnaryServerInfo, handler grpc.UnaryHandler) (interface{}, error) {
    resp, err := handler(ctx, req)
    if err != nil {
        // Log the error with context
        log.Printf("gRPC error in %s: %v", info.FullMethod, err)
        
        // You can also modify the error here
        if isInternalError(err) {
            return nil, status.Error(codes.Internal, "Something went wrong")
        }
    }
    return resp, err
}
```

## Related Articles

If you're interested in learning more about gRPC testing, check out these related articles from the Keploy:

1. [**Understanding gRPC: A Complete Guide For Modern Developers**](https://keploy.io/blog/community/what-is-grpc) **\-** In this blog, you will learn everything about gRPC.
    
2. [**Go Mocks and Stubs Made Easy**](https://keploy.io/blog/technology/go-mocks-and-stubs-made-easy) - Learn how Keploy simplifies the process of creating mocks and stubs for Go applications, including gRPC services.
    
3. [**Capture Grpc Traffic Going Out From A Server**](https://keploy.io/blog/technology/capture-grpc-traffic-going-out-from-a-server) **\-** In this blog, you will capture (log) the request and response from a gRPC client and server
    

## Conclusion

gRPC error codes may look daunting for the first time, but they are really reasonable once you get the patterns. The trick is to think of them as a means of communication between your services and they communicate to you what went wrong and usually hint at how to solve it.

Keep in mind that good error handling is what makes the amateur project different from production ready systems. So, spend some time to handle these errors graciously.

## Frequently Asked Questions

### 1\. What's the difference between DEADLINE\_EXCEEDED and CANCELLED?

Look, `DEADLINE_EXCEEDED` means your call timed out, it took longer than the configured deadline. `CANCELLED` means someone (usually the client) actively cancelled the operation before it was completed. Think of it like this: a deadline exceeded is like a timer going off, while cancelled is like pressing the stop button.

### 2\. Should I retry all gRPC errors?

No! Here's my rule of thumb:

* **Retry these**: `UNAVAILABLE`, `DEADLINE_EXCEEDED`, `RESOURCE_EXHAUSTED`, `ABORTED`
    
* **Don't retry these**: `INVALID_ARGUMENT`, `NOT_FOUND`, `PERMISSION_DENIED`, `UNAUTHENTICATED`, `FAILED_PRECONDITION`
    
* **Maybe retry once**: `UNKNOWN`, `INTERNAL`
    

The key is understanding whether the error is transient (temporary) or permanent.

### 3\. How do I convert HTTP status codes to gRPC error codes?

While there's no perfect mapping to convert the status codes but, here are common conversions:

* HTTP 400 → `INVALID_ARGUMENT`
    
* HTTP 401 → `UNAUTHENTICATED`
    
* HTTP 403 → `PERMISSION_DENIED`
    
* HTTP 404 → `NOT_FOUND`
    
* HTTP 409 → `ALREADY_EXISTS`
    
* HTTP 429 → `RESOURCE_EXHAUSTED`
    
* HTTP 500 → `INTERNAL`
    
* HTTP 503 → `UNAVAILABLE`
    

### 4\. Can I create custom error codes?

Nope! gRPC error codes are standardised, and you can't add your own. However, you can use the error message and details (in the richer error model) to provide specific information. This standardization is actually a good thing, it means error codes mean the same thing across all gRPC services.

### 5\. Is there a difference between gRPC error codes across different programming languages?

The error codes themselves are identical across all languages and that's the beauty of gRPC's standardisation! However, how you handle them varies:

* **Go**: Uses `google.golang.org/grpc/codes` and `status` packages
    
* **Java**: Uses `io.grpc.Status` and `StatusRuntimeException`
    
* **Python**: Uses `grpc.StatusCode` and `grpc.RpcError`
    
* **C++**: Uses `grpc::StatusCode` and `grpc::Status`
    

The underlying meaning and numeric values are the same, but the API for checking and creating errors differs.