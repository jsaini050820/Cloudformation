# Cloudformation
Cloudforrnation Templates
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Resources": {
    "vpc05dea06d7a0bf085c": {
      "Type": "AWS::EC2::VPC",
      "Properties": {
        "CidrBlock": "10.0.0.0/16",
        "InstanceTenancy": "default",
        "EnableDnsSupport": "true",
        "EnableDnsHostnames": "false",
        "Tags": [
          {
            "Key": "Name",
            "Value": "Jaspal"
          }
        ]
      }
    },
    "subnet09ee0fe4ee3959cd2": {
      "Type": "AWS::EC2::Subnet",
      "Properties": {
        "CidrBlock": "10.0.1.0/24",
        "AvailabilityZone": "us-west-2a",
        "VpcId": {
          "Ref": "vpc05dea06d7a0bf085c"
        },
        "Tags": [
          {
            "Key": "Name",
            "Value": "PublicSubnet"
          }
        ]
      }
    },
    "igw0ed5e740fa3ea3761": {
      "Type": "AWS::EC2::InternetGateway",
      "Properties": {
        "Tags": [
          {
            "Key": "Name",
            "Value": "IG"
          }
        ]
      }
    },
    "dopt8b31b7f3": {
      "Type": "AWS::EC2::DHCPOptions",
      "Properties": {
        "DomainName": "us-west-2.compute.internal",
        "DomainNameServers": [
          "AmazonProvidedDNS"
        ]
      }
    },
    "acl06b6e75cb3fe312be": {
      "Type": "AWS::EC2::NetworkAcl",
      "Properties": {
        "VpcId": {
          "Ref": "vpc05dea06d7a0bf085c"
        }
      }
    },
    "rtb059e8a0a03225807c": {
      "Type": "AWS::EC2::RouteTable",
      "Properties": {
        "VpcId": {
          "Ref": "vpc05dea06d7a0bf085c"
        },
        "Tags": [
          {
            "Key": "Name",
            "Value": "RT"
          }
        ]
      }
    },
    "rtb0220634ce8bc1929b": {
      "Type": "AWS::EC2::RouteTable",
      "Properties": {
        "VpcId": {
          "Ref": "vpc05dea06d7a0bf085c"
        }
      }
    },
    "instancei011ed0d1ed01d617e": {
      "Type": "AWS::EC2::Instance",
      "Properties": {
        "DisableApiTermination": "false",
        "InstanceInitiatedShutdownBehavior": "stop",
        "ImageId": "ami-04590e7389a6e577c",
        "InstanceType": "t2.micro",
        "KeyName": "mona",
        "Monitoring": "false",
        "Tags": [
          {
            "Key": "Name",
            "Value": "WebServer"
          }
        ],
        "NetworkInterfaces": [
          {
            "DeleteOnTermination": "true",
            "Description": "Primary network interface",
            "DeviceIndex": 0,
            "SubnetId": {
              "Ref": "subnet09ee0fe4ee3959cd2"
            },
            "PrivateIpAddresses": [
              {
                "PrivateIpAddress": "10.0.1.176",
                "Primary": "true"
              }
            ],
            "GroupSet": [
              {
                "Ref": "sgtestvpcjaspalSG"
              }
            ]
          }
        ]
      }
    },
    "sgtestvpcjaspalSG": {
      "Type": "AWS::EC2::SecurityGroup",
      "Properties": {
        "GroupDescription": "web server sg",
        "VpcId": {
          "Ref": "vpc05dea06d7a0bf085c"
        }
      }
    },
    "acl1": {
      "Type": "AWS::EC2::NetworkAclEntry",
      "Properties": {
        "CidrBlock": "0.0.0.0/0",
        "Egress": "true",
        "Protocol": "-1",
        "RuleAction": "allow",
        "RuleNumber": "100",
        "NetworkAclId": {
          "Ref": "acl06b6e75cb3fe312be"
        }
      }
    },
    "acl2": {
      "Type": "AWS::EC2::NetworkAclEntry",
      "Properties": {
        "CidrBlock": "0.0.0.0/0",
        "Protocol": "-1",
        "RuleAction": "allow",
        "RuleNumber": "100",
        "NetworkAclId": {
          "Ref": "acl06b6e75cb3fe312be"
        }
      }
    },
    "subnetacl1": {
      "Type": "AWS::EC2::SubnetNetworkAclAssociation",
      "Properties": {
        "NetworkAclId": {
          "Ref": "acl06b6e75cb3fe312be"
        },
        "SubnetId": {
          "Ref": "subnet09ee0fe4ee3959cd2"
        }
      }
    },
    "gw1": {
      "Type": "AWS::EC2::VPCGatewayAttachment",
      "Properties": {
        "VpcId": {
          "Ref": "vpc05dea06d7a0bf085c"
        },
        "InternetGatewayId": {
          "Ref": "igw0ed5e740fa3ea3761"
        }
      }
    },
    "subnetroute1": {
      "Type": "AWS::EC2::SubnetRouteTableAssociation",
      "Properties": {
        "RouteTableId": {
          "Ref": "rtb059e8a0a03225807c"
        },
        "SubnetId": {
          "Ref": "subnet09ee0fe4ee3959cd2"
        }
      }
    },
    "route1": {
      "Type": "AWS::EC2::Route",
      "Properties": {
        "DestinationCidrBlock": "0.0.0.0/0",
        "RouteTableId": {
          "Ref": "rtb059e8a0a03225807c"
        },
        "GatewayId": {
          "Ref": "igw0ed5e740fa3ea3761"
        }
      },
      "DependsOn": "gw1"
    },
    "dchpassoc1": {
      "Type": "AWS::EC2::VPCDHCPOptionsAssociation",
      "Properties": {
        "VpcId": {
          "Ref": "vpc05dea06d7a0bf085c"
        },
        "DhcpOptionsId": {
          "Ref": "dopt8b31b7f3"
        }
      }
    },
    "ingress1": {
      "Type": "AWS::EC2::SecurityGroupIngress",
      "Properties": {
        "GroupId": {
          "Ref": "sgtestvpcjaspalSG"
        },
        "IpProtocol": "tcp",
        "FromPort": "80",
        "ToPort": "80",
        "CidrIp": "0.0.0.0/0"
      }
    },
    "egress1": {
      "Type": "AWS::EC2::SecurityGroupEgress",
      "Properties": {
        "GroupId": {
          "Ref": "sgtestvpcjaspalSG"
        },
        "IpProtocol": "-1",
        "CidrIp": "0.0.0.0/0"
      }
    }
  },
  "Description": "Existing VPC Web Server"
}
