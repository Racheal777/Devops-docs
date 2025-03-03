SigNoz Instrumentation Setup Guide

🌟 Introduction SigNoz is an open-source observability platform that helps you monitor your application’s performance using OpenTelemetry. This guide walks you through setting up SigNoz instrumentation for your Python application in a clear and easy-to-follow manner.

🛠️ Step 1: Install and Run SigNoz using Docker To get started, you need to install and run SigNoz using Docker. Follow these steps: Download the Docker Compose file from the official SigNoz website. Run the container following the provided instructions. Once the setup is complete, visit the following link to select the setup guide for your programming language: 🔗 SigNoz Instrumentation Guide

Step 2: Setting Up OpenTelemetry for Python After setting up SigNoz, the next step is configuring OpenTelemetry for your Python application. ✅ Install Required Packages Run the following command to install OpenTelemetry and the required exporter: For a Python project, once you have your virtual environment, the next thing is to install OpenTelemetry.

pip install opentelemetry-distro==0.43b0 pip install opentelemetry-exporter-otlp==1.22.0

🎯 What These Packages Do: opentelemetry-distro: Provides an easy way to configure OpenTelemetry automatically. opentelemetry-exporter-otlp: Enables your application to send telemetry data to SigNoz.

🚀 Step 3: Enable Automatic Instrumentation OpenTelemetry provides an easy way to automatically instrument your application. Run the following command: Add automatic instrumentation The below command inspects the dependencies of your application and installs the instrumentation packages relevant for your Python application. opentelemetry-bootstrap --action=install

This command inspects your dependencies and installs the necessary instrumentation packages for your Python application.

▶️ Step 4: Run Your Application with OpenTelemetry To start your application with OpenTelemetry instrumentation, use the following command:

OTEL_RESOURCE_ATTRIBUTES=service.name=finance_api OTEL_EXPORTER_OTL P_ENDPOINT="http://localhost:4317" OTEL_EXPORTER_OTLP_PROTOCOL=grpc opentelemetry-instrument fastapi run main.py

⚠️ Important Notes: Replace finance_api with your actual service name. Do not run the application in development mode (--reload), as it may cause issues.

📊 Step 5: Monitor Your Application on SigNoz Once your application is running, you can access the SigNoz dashboard: Open SigNoz UI Navigate to: http://localhost:3301 View Service Metrics Click on your service name in the SigNoz dashboard. Check traces to see request flows. View logs to monitor application behavior.

Click the Service name on the signoz application

On the side, you can check the traces, which should look something like this

You also have access to logs to monitor your application logs

🎉 Conclusion You have successfully set up SigNoz instrumentation for your Python application! Now you can monitor your application’s performance, track traces, and analyze logs efficiently. 🚀 Next Steps: Explore more instrumentation options. Configure custom metrics. Optimize application performance using SigNoz insights. 🔗 Useful Links: SigNoz Documentation OpenTelemetry Python

🔹 Happy Monitoring! 🚀