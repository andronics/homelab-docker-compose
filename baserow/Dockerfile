FROM docker.n8n.io/n8nio/n8n:latest
USER root

RUN \
    echo "Install Node Package: Embedded JavaScript Templates (EJS)" && \
    npm install -g ejs && \
    \
    echo "Install Node Package: Handlebars.js" && \
    npm install -g handlebars && \
    \
    echo "Install Node Package: Pug" && \
    npm install -g pug && \
    \
    echo "Overwriting License: isFeatureEnabled" && \
    sed -i "/return this\.manager?.hasFeatureEnabled(feature) ?? false;/s/.*/return (feature == constants_1.LICENSE_FEATURES.API_DISABLED || feature == constants_1.LICENSE_FEATURES.SHOW_NON_PROD_BANNER) ? 0 : -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Advanced Execution Filters" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.ADVANCED_EXECUTION_FILTERS);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Advanced Permissions" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.ADVANCED_PERMISSIONS);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: AI Assitant" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.AI_ASSISTANT);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: AI Credits" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.AI_CREDITS);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: API Disable" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.API_DISABLED);/s/.*/return 0;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Binary Data S3" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.BINARY_DATA_S3);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Custom Registry" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.COMMUNITY_NODES_CUSTOM_REGISTRY);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Debug In Editor" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.DEBUG_IN_EDITOR);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: External Secrets" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.EXTERNAL_SECRETS);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: LDAP" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.LDAP);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Log Streaming" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.LOG_STREAMING);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Multi Main" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.MULTIPLE_MAIN_INSTANCES);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Project Role Admin" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.PROJECT_ROLE_ADMIN);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Project Role Editor" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.PROJECT_ROLE_EDITOR);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Project Role Viewer" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.PROJECT_ROLE_VIEWER);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Sharing" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.SHARING);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: SAML" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.SAML);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Source Control" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.SOURCE_CONTROL);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Variables" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.VARIABLES);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Worker View" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.WORKER_VIEW);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting License Feature: Workflow History" && \
    sed -i "/return this\.isFeatureEnabled(constants_1.LICENSE_FEATURES\.WORKFLOW_HISTORY);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting Feature: All" && \
    sed -i "/return this\.manager?.getFeatureValue(feature);/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js && \
    \
    echo "Overwriting Plan Name: Workflow History" && \
    sed -i "/return this\.getFeatureValue('planName') ?? 'Enterprise';/s/.*/return -1;/" /usr/local/lib/node_modules/n8n/dist/license.js

USER node