pipeline {
  agent none
  parameters {
    booleanParam (
      defaultValue: true,
      description: '',
      name: 'linux_64'
    )
    booleanParam (
      defaultValue: true,
      description: '',
      name: 'win_64'
    )
    booleanParam (
      defaultValue: false,
      description: '',
      name: 'documentserver'
    )
    booleanParam (
      defaultValue: false,
      description: '',
      name: 'documentserver_de'
    )
    booleanParam (
      defaultValue: true,
      description: '',
      name: 'documentserver_ie'
    )
  }
  triggers {
    cron('H 20 * * *')
  }
  stages {
    stage('Prepare') {
      steps {
        script {
          def branchName = env.BRANCH_NAME
          def productVersion = "5.4.99"
          def pV = branchName =~ /^(release|hotfix)\\/v(.*)$/
          if(pV.find()) {
            productVersion = pV.group(2)
          }
          env.PRODUCT_VERSION = productVersion
          env.APP_TITLE_TEXT = "Р7-Офис"
          env.API_URL_EDITING_CALLBACK = "http://helpcenter.r7-office.ru/api/editors/callback.aspx"
          env.COEDITING_DESKTOP = "Совместная работа"
          env.PLUGIN_LINK = "http://helpcenter.r7-office.ru/api/plugins/index.aspx"
          env.PLUGIN_LINK_MACROS = "http://helpcenter.r7-office.ru/api/plugins/macros.aspx"
          env.HELP_URL = "http://helpcenter.r7-office.ru"
          env.PUBLISHER_ADDRESS = "Россия, 603152, г. Нижний Новгород, ул. Ларина, д. 22 лит. Д."
          env.PUBLISHER_PHONE = "+7 831 422 48 30"
          env.PUBLISHER_URL = "http://r7-office.ru/"
          env.SUPPORT_EMAIL = "support@r7-office.ru"
          env.SUPPORT_MAIL = "support@r7-office.ru"
          env.SUPPORT_URL = "http://helpcenter.r7-office.ru"
          env.SALES_EMAIL = "sales@r7-office.ru"
          env.COMPANY_NAME = "R7-Office"
        }
      }
    }
    stage('Build') {
      parallel {
        stage('DocumentServer-IE(linux_64) build') {
          agent { label 'linux_64' }
          environment {
            PRODUCT_NAME = "documentserver-ie"
            PUBLISHER_NAME = "AO \\\"NOVYE KOMMUNIKACIONNYE TEHNOLOGII\\\""
            APP_COPYRIGHT = "Copyright (C) \\\"NOVYE KOMMUNIKACIONNYE TEHNOLOGII\\\" 2019. All rights reserved"
          }
          steps {
            script {
              def utils = load "utils.groovy"
              if ( params.linux_64 && params.documentserver_ie) {
                utils.linuxBuild(env.BRANCH_NAME)
                /*
                utils.tagRepos("v${env.PRODUCT_VERSION}.${env.BUILD_NUMBER}")
                */
              }
            }
          }
        }
        stage('DocumentServer(linux_64) build') {
          agent { label 'linux_64' }
          environment { PRODUCT_NAME = "documentserver" }
          steps {
            script {
              def utils = load "utils.groovy"
              if ( params.linux_64 && params.documentserver) {
                utils.linuxBuild(env.BRANCH_NAME )
              }
            }
          }
        }
        stage('DocumentServer-DE(linux_64) build') {
          agent { label 'linux_64' }
          environment { PRODUCT_NAME = "documentserver-de" }
          steps {
            script {
              def utils = load "utils.groovy"
              if ( params.linux_64 && params.documentserver_de) {
                utils.linuxBuild(env.BRANCH_NAME)
              }
            }
          }
        }
        stage('DocumentServer(win_64) build') {
          agent {
            node {
              label 'win_64'
              customWorkspace "C:\\rds\\${env.BRANCH_NAME}\\os"
            }
          }
          environment { PRODUCT_NAME = "DocumentServer" }
          steps {
            script {
              def utils = load "utils.groovy"
              if ( params.win_64 && params.documentserver ) {
                utils.windowsBuild(env.BRANCH_NAME)
              }
            }
          }
        }
        stage('DocumentServer-DE(win_64) build') {
          agent {
            node {
              label 'win_64'
              customWorkspace "C:\\rds\\${env.BRANCH_NAME}\\de"
            }
          }
          environment { PRODUCT_NAME = "DocumentServer-DE" }
          steps {
            script {
              def utils = load "utils.groovy"
              if ( params.win_64 && params.documentserver_de ) {
                utils.windowsBuild(env.BRANCH_NAME)
              }
            }
          }
        }
        stage('DocumentServer-IE(win_64) build') {
          agent {
            node {
              label 'win_64'
              customWorkspace "C:\\rds\\${env.BRANCH_NAME}\\ie"
            }
          }
          environment {
            PRODUCT_NAME = "DocumentServer-IE"
            PUBLISHER_NAME = "AO \\\"NOVYE KOMMUNIKACIONNYE TEHNOLOGII\\\""
            APP_COPYRIGHT = "Copyright (C) AO \\\"NOVYE KOMMUNIKACIONNYE TEHNOLOGII\\\" 2019. All rights reserved"
          }
          steps {
            script {
              def utils = load "utils.groovy"
              if ( params.win_64 && params.documentserver_ie ) {
                utils.windowsBuild(env.BRANCH_NAME)
              }
            }
          }
        }
      }
    }
  }
}
