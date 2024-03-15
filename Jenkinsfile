pipeline {
    agent any

    stages {
        stage('Setup parameters') {
            steps { 
                script {
                    properties([
                        parameters([
                            choice(
                                description: "The AWS account where the ElastiCache Redis will be deployed.",
                                name: 'DeploymentAccount',
                                choices: ['Please select','NVSGISRSACN','NVSGISRSBCN','NVSGISRSICN','NVSGISRSIDCN','NVSGISRCCCN']
                            ),
                            choice(
                                description: "Please select the environment.",
                                name: 'Environment',
                                choices: ['GB','TST']
                            ),
                            string(
                                description: "Plesae specifiy the description of this Redis",
                                name: 'ReplicationGroupDescription'
                            ),
                            choice(
                                description: "Version compatibility of the Redis engine that will run on your nodes.",
                                name: 'EngineVersion',
                                choices: ['4.0.10','5.0.6','6.0','6.2','7.0','7.1'],
                                defaultValue: '7.1'
                            ),
                            choice(
                                description: "Please select the Parameter Group for the Redis. Parameter groups control the runtime properties of your nodes and clusters. Please note you need to select the corresponding version with Engine Version..",
                                name: 'CacheParameterGroupName',
                                choices: ['default.redis4.0','default.redis4.0.cluster.on','default.redis5.0','default.redis5.0.cluster.on','default.redis6.x','default.redis6.x.cluster.on','default.redis7','default.redis7.cluster.on'],
                                defaultValue: 'default.redis7'
                            ),
                            choice(
                                description: "Please select the instance type of the Redis nodes.",
                                name: 'CacheNodeType',
                                choices: ['cache.m7g.large','cache.m7g.xlarge','cache.m7g.2xlarge','cache.m7g.4xlarge','cache.m7g.8xlarge','cache.m7g.12xlarge','cache.m7g.16xlarge','cache.m6g.large','cache.m6g.xlarge','cache.m6g.2xlarge','cache.m6g.4xlarge','cache.m6g.8xlarge','cache.m6g.12xlarge','cache.m6g.16xlarge','cache.m5.large','cache.m5.xlarge','cache.m5.2xlarge','cache.m5.4xlarge','cache.m5.12xlarge','cache.m5.24xlarge','cache.m4.large','cache.m4.xlarge','cache.m4.2xlarge','cache.m4.4xlarge','cache.m4.10xlarge','cache.t4g.micro','cache.t4g.small','cache.t4g.medium','cache.t3.micro','cache.t3.small','cache.t3.medium','cache.t2.micro','cache.t2.small','cache.t2.medium','cache.r7g.large','cache.r7g.xlarge','cache.r7g.2xlarge','cache.r7g.4xlarge','cache.r7g.8xlarge','cache.r7g.12xlarge','cache.r7g.16xlarge','cache.r6g.large','cache.r6g.xlarge','cache.r6g.2xlarge','cache.r6g.4xlarge','cache.r6g.8xlarge','cache.r6g.12xlarge','cache.r6g.16xlarge','cache.r5.large','cache.r5.xlarge','cache.r5.2xlarge','cache.r5.4xlarge','cache.r5.12xlarge','cache.r5.24xlarge','cache.r4.large','cache.r4.xlarge','cache.r4.2xlarge','cache.r4.4xlarge','cache.r4.8xlarge','cache.r4.16xlarge','cache.r6gd.xlarge','cache.r6gd.2xlarge','cache.r6gd.4xlarge','cache.r6gd.8xlarge','cache.r6gd.12xlarge','cache.r6gd.16xlarge','cache.c7gn.large','cache.c7gn.xlarge']
                            ),
                            string(
                                description: "Enter the number of shards in this cluster, from 1 to 500.",
                                name: 'NumShards',
                                defaultValue: "1"
                            ),
                            choice(
                                description: "Replicas per shard. Select the number of replicas for each shard, from 0 to 5.",
                                name: 'NumReplicas',
                                choices: ['0','1','2','3','4','5'],
                                defaultValue: '2'
                            ),
                            string(
                                description: "Please input the KMS Key ARN for encrytion at rest. For example, arn:aws-cn:kms:cn-north-1:988109322633:key/cb1fc7f4-5745-42e1-b6e6-d813c4d0e635.",
                                name: 'KmsKeyId'
                            ),
                            string(
                                description: "Please input the Secret ARN for encryption in transit. For example, arn:aws-cn:secretsmanager:cn-north-1:988109322633:secret:SCRT-CNBJ-RSBCN-DEV-001-PB97cu",
                                name: 'AuthToken'
                            ),
                            string(
                                description: "Provide the Accesskey of China RCC GB RRCCCN_AWS_JENKINS Role",
                                name: 'AWSAccessKey'                                
                            ),
                            password(
                                description: "Provide the Secret Accesskey of China RCC GB RRCCCN_AWS_JENKINS Role",
                                name: 'AWSSecretKey'
                            ),
                            password(
                                description: "Provide the Session Token of China RCC GB RRCCCN_AWS_JENKINS Role",
                                name: 'AWSSessionToken'
                            ),
                            choice(
                                description: "Provide the AWS Region",
                                name: 'RegionName',
                                choices: ['cn-north-1']
                            ),
                            choice(
                                description: "Services to be deployed",
                                name: 'ServicesToBeDeployed',
                                choices: ['redis']
                            )
                        ])
                    ])
                }
            }
        }
        stage('Checkout templates') {
            steps {
                    cleanWs()
                    checkout scm
                    checkout scm: [
                    $class: 'GitSCM',
                    userRemoteConfigs: [
                        [
                            url: "ssh://git@bitbucketenterprise.aws.novartis.net/tispucengg/pucengg_china_shared.git",
                            credentialsId: "BitbucketKey"
                        ]
                    ],
                    branches: [[name: 'feature/CLIN-28025-Redis']],
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'aws-redis']]
                ]
            }
        }        
        stage('Deploy stack') {
            steps {
                    script {
                        sh "bash deploy_stack.sh"
                    }
            }
        }        
    }
}
