<dependencies>
    <!-- SLF4J API -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.36</version>
    </dependency>

    <!-- SLF4J Binding for Log4j 2 -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-slf4j-impl</artifactId>
        <version>2.19.0</version>
    </dependency>

    <!-- Log4j 2 Core -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.19.0</version>
    </dependency>

    <!-- Log4j 2 API -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>2.19.0</version>
    </dependency>
</dependencies>



<Configuration status="WARN">
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{36} - %msg%n" />
        </Console>
    </Appenders>
    <Loggers>
        <!-- Logger for Jersey -->
        <Logger name="org.glassfish.jersey" level="debug" additivity="false">
            <AppenderRef ref="Console"/>
        </Logger>

        <!-- Logger for Jetty -->
        <Logger name="org.eclipse.jetty" level="debug" additivity="false">
            <AppenderRef ref="Console"/>
        </Logger>

        <Root level="info">
            <AppenderRef ref="Console"/>
        </Root>
    </Loggers>
</Configuration>


import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.ws.rs.NotSupportedException;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.Response;
import javax.ws.rs.ext.ExceptionMapper;
import javax.ws.rs.ext.Provider;

@Provider
public class UnsupportedMediaTypeExceptionMapper implements ExceptionMapper<NotSupportedException> {

    private static final Logger logger = LoggerFactory.getLogger(UnsupportedMediaTypeExceptionMapper.class);

    @Override
    public Response toResponse(NotSupportedException exception) {
        // Log the error details
        logger.error("415 Unsupported Media Type error encountered: {}", exception.getMessage());

        // Optionally, log the stack trace for deeper debugging
        logger.debug("Stack trace: ", exception);

        // Log any other details you want here, such as custom headers, user info, etc.
        
        // Build a response to return to the client
        return Response.status(Response.Status.UNSUPPORTED_MEDIA_TYPE) // 415 status code
                       .type(MediaType.APPLICATION_JSON) // Response content type
                       .entity("{\"error\":\"Unsupported Media Type. Please provide the correct Content-Type.\"}")
                       .build();
    }
}
