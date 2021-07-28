

https://springdoc.org/#how-can-use-custom-jsonyml-file-instead-of-generated-one

https://springdoc.org/#what-is-a-proper-way-to-set-up-swagger-ui-to-use-provided-spec-yml



``` json
{
  "openapi": "3.0.1",
  "info": {
    "title": "Lisence Manager Service",
    "description": "Lisence Manager Service for SecuLetter Service",
    "contact": {
      "name": "API Support",
      "email": "raecheol.park@seculetter.com"
    },
    "version": "0.0.1"
  },
  "servers": [
    {
      "url": "http://localhost:8080",
      "description": "localhost server"
    }
  ],
  "tags": [
    {
      "name": "user",
      "description": "User resource"
    },
    {
      "name": "license",
      "description": "license resource"
    },
    {
      "name": "license-inquire",
      "description": "license-inquire resource to make license"
    }
  ],
  "paths": {
    "/users": {
      "post": {
        "operationId": "Add new user",
        "tags": [
          "user"
        ],
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UserPostRequestDTO"
              }
            }
          }
        },
        "responses": {
          "201": {
            "description": "Created",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/UserResponseDTO"
                }
              }
            }
          }
        }
      },
      "get": {
        "operationId": "Get all user",
        "tags": [
          "user"
        ],
        "parameters": [
          {
            "$ref": "#/components/parameters/page"
          },
          {
            "$ref": "#/components/parameters/size"
          }
        ],
        "responses": {
          "200": {
            "description": "OK",
            "content": {
              "application/json": {
                "schema": {
                  "type": "array",
                  "items": {
                    "$ref": "#/components/schemas/UserResponseDTO"
                  }
                }
              }
            }
          }
        }
      }
    },
    "/users/{user-id}": {
      "parameters": [
        {
          "name": "user-id",
          "in": "path",
          "description": "ID of user",
          "required": true,
          "schema": {
            "type": "number",
            "format": "int64",
            "writeOnly": true
          }
        }
      ],
      "get": {
        "operationId": "Get user by id",
        "tags": [
          "user"
        ],
        "responses": {
          "200": {
            "description": "OK",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/UserResponseDTO"
                }
              }
            }
          }
        }
      },
      "patch": {
        "operationId": "PATCH user by id",
        "tags": [
          "user"
        ],
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "type": "object",
                "properties": {
                  "password": {
                    "type": "string",
                    "format": "password",
                    "writeOnly": true,
                    "example": "new-secret-password"
                  },
                  "authority": {
                    "$ref": "#/components/schemas/authority"
                  }
                }
              }
            }
          }
        },
        "responses": {
          "200": {
            "description": "OK",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/UserResponseDTO"
                }
              }
            }
          }
        }
      },
      "delete": {
        "operationId": "Delete user by id",
        "tags": [
          "user"
        ],
        "responses": {
          "204": {
            "description": "NO CONTENT"
          }
        }
      }
    },
    "/licenses": {
      "post": {
        "operationId": "Add new license",
        "tags": [
          "license"
        ],
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/LicensePostRequestDTO"
              }
            }
          }
        },
        "responses": {
          "201": {
            "$ref": "#/components/responses/license-created"
          }
        }
      },
      "get": {
        "operationId": "Get licenses",
        "tags": [
          "license"
        ],
        "parameters": [
          {
            "$ref": "#/components/parameters/page"
          },
          {
            "$ref": "#/components/parameters/size"
          }
        ],
        "responses": {
          "200": {
            "description": "OK",
            "content": {
              "application/json": {
                "schema": {
                  "type": "array",
                  "items": {
                    "$ref": "#/components/schemas/LisenceResponseDTO"
                  }
                }
              }
            }
          }
        }
      }
    },
    "/licenses/{license-id}": {
      "parameters": [
        {
          "name": "license-id",
          "in": "path",
          "description": "ID of license",
          "required": true,
          "schema": {
            "type": "string",
            "writeOnly": true
          }
        }
      ],
      "get": {
        "operationId": "Get license",
        "tags": [
          "license"
        ],
        "responses": {
          "200": {
            "description": "OK",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/LisenceResponseDTO"
                }
              }
            }
          }
        }
      },
      "delete": {
        "operationId": "Delete license",
        "tags": [
          "license"
        ],
        "responses": {
          "204": {
            "description": "NO CONTENT",
            "content": {}
          }
        }
      }
    },
    "/licenseInquires": {
      "post": {
        "operationId": "Add new license inquiry",
        "tags": [
          "license-inquire"
        ],
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/LisenceInquirePostRequestDTO"
              }
            }
          }
        },
        "responses": {
          "201": {
            "description": "Created",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/LisenceResponseDTO"
                }
              }
            }
          }
        }
      },
      "get": {
        "operationId": "Get licenseInquires",
        "tags": [
          "license-inquire"
        ],
        "parameters": [
          {
            "$ref": "#/components/parameters/page"
          },
          {
            "$ref": "#/components/parameters/size"
          }
        ],
        "responses": {
          "201": {
            "description": "OK",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/LicenseInquireResponseDTO"
                }
              }
            }
          }
        }
      }
    },
    "/licenseInquires/{license-inquire-id}": {
      "parameters": [
        {
          "$ref": "#/components/parameters/license-inquire-id"
        }
      ],
      "get": {
        "operationId": "Get licenseInquire",
        "tags": [
          "license-inquire"
        ],
        "responses": {
          "200": {
            "description": "OK",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/LicenseInquireResponseDTO"
                }
              }
            }
          }
        }
      },
      "delete": {
        "operationId": "Delete licenseInquires",
        "tags": [
          "license-inquire"
        ],
        "responses": {
          "204": {
            "description": "NO CONTENT",
            "content": {}
          }
        }
      }
    },
    "/licenseInquires/{license-inquire-id}/approve": {
      "parameters": [
        {
          "$ref": "#/components/parameters/license-inquire-id"
        }
      ],
      "post": {
        "operationId": "Approve license inquire",
        "tags": [
          "license",
          "license-inquire"
        ],
        "responses": {
          "201": {
            "$ref": "#/components/responses/license-created"
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "LicensePostRequestDTO": {
        "type": "object",
        "writeOnly": true,
        "properties": {
          "expire_date": {
            "$ref": "#/components/schemas/expireDate"
          },
          "customor": {
            "$ref": "#/components/schemas/UserResponseDTO"
          },
          "inquirer": {
            "$ref": "#/components/schemas/UserResponseDTO"
          }
        },
        "required": [
          "expired_date",
          "customor",
          "inquirer"
        ]
      },
      "LisenceResponseDTO": {
        "type": "object",
        "readOnly": true,
        "properties": {
          "expire_date": {
            "$ref": "#/components/schemas/expireDate"
          },
          "message": {
            "type": "string",
            "description": "Message about this license",
            "example": "Some message about this license"
          },
          "customor": {
            "$ref": "#/components/schemas/UserResponseDTO"
          },
          "inquirer": {
            "$ref": "#/components/schemas/UserResponseDTO"
          },
          "creator": {
            "$ref": "#/components/schemas/UserResponseDTO"
          },
          "_links": {
            "$ref": "#/components/schemas/license_links"
          }
        },
        "example": {
          "expire_date": 1651532065,
          "message": "Some message about this license",
          "customor": {
            "email": "some-user@gmail.com",
            "authority": "USER",
            "_links": {
              "self": {
                "href": "http://localhost:8080/users/{user-id}"
              }
            }
          },
          "inquirer": {
            "email": "some-partner@seculetter.com",
            "authority": "INQUIRER",
            "_links": {
              "self": {
                "href": "http://localhost:8080/users/{user-id}"
              }
            }
          },
          "creator": {
            "email": "raecheol.park@seculetter.com",
            "authority": "ADMIN",
            "_links": {
              "self": {
                "href": "http://localhost:8080/users/{user-id}"
              }
            }
          },
          "_links": {
            "self": {
              "href": "http://localhost:8080/users/{license-id}"
            }
          }
        }
      },
      "LisenceInquirePostRequestDTO": {
        "type": "object",
        "writeOnly": true,
        "properties": {
          "expire_date": {
            "$ref": "#/components/schemas/expireDate"
          },
          "customor": {
            "$ref": "#/components/schemas/customor"
          }
        },
        "required": [
          "expire_date",
          "customor"
        ]
      },
      "LicenseInquireResponseDTO": {
        "type": "object",
        "readOnly": true,
        "properties": {
          "accepted": {
            "type": "boolean",
            "default": false,
            "description": "If license inquire accepted, true else false"
          },
          "expire_date": {
            "$ref": "#/components/schemas/expireDate"
          },
          "customor": {
            "$ref": "#/components/schemas/customor"
          },
          "inquirer": {
            "$ref": "#/components/schemas/inquirer"
          },
          "_links": {
            "$ref": "#/components/schemas/licenseInquire_links"
          }
        },
        "example": {
          "accepted": false,
          "expire_date": 1651532065,
          "message": "Some message about this license",
          "customor": {
            "email": "some-user@gmail.com",
            "authority": "USER",
            "_links": {
              "self": {
                "href": "http://localhost:8080/users/{user-id}"
              }
            }
          },
          "inquirer": {
            "email": "some-partner@seculetter.com",
            "authority": "INQUIRER",
            "_links": {
              "self": {
                "href": "http://localhost:8080/users/{user-id}"
              }
            }
          },
          "_links": {
            "self": {
              "href": "http://localhost:8080/licenseInquires/{licenseInquire-id}"
            }
          }
        }
      },
      "UserPostRequestDTO": {
        "type": "object",
        "properties": {
          "email": {
            "$ref": "#/components/schemas/email"
          },
          "password": {
            "type": "string",
            "format": "password",
            "description": "Password of user",
            "example": "some secret of user"
          },
          "authority": {
            "$ref": "#/components/schemas/authority"
          }
        },
        "writeOnly": true,
        "required": [
          "email",
          "password",
          "authority"
        ]
      },
      "UserResponseDTO": {
        "type": "object",
        "readOnly": true,
        "properties": {
          "email": {
            "$ref": "#/components/schemas/email"
          },
          "authority": {
            "$ref": "#/components/schemas/authority"
          },
          "_links": {
            "$ref": "#/components/schemas/user_links"
          }
        }
      },
      "expireDate": {
        "type": "integer",
        "format": "int64",
        "description": "Expire date for license",
        "example": 1651532065
      },
      "customor": {
        "$ref": "#/components/schemas/UserResponseDTO"
      },
      "inquirer": {
        "$ref": "#/components/schemas/UserResponseDTO"
      },
      "creator": {
        "$ref": "#/components/schemas/UserResponseDTO"
      },
      "email": {
        "type": "string",
        "format": "email",
        "pattern": "^[^@\\s]+@[^@\\s]+\\.[^@\\s]+$",
        "description": "Email address",
        "example": "raecheol.park@seculetter.com"
      },
      "authority": {
        "type": "string",
        "enum": [
          "USER",
          "PARTNER",
          "ADMIN"
        ],
        "description": "User authority",
        "example": "USER"
      },
      "user_links": {
        "type": "object",
        "readOnly": true,
        "properties": {
          "self": {
            "type": "object",
            "readOnly": true,
            "properties": {
              "href": {
                "type": "string",
                "format": "url",
                "description": "URL to retrieve user",
                "example": "http://localhost:8080/users/{user-id}"
              }
            }
          }
        }
      },
      "license_links": {
        "type": "object",
        "readOnly": true,
        "properties": {
          "self": {
            "type": "object",
            "readOnly": true,
            "properties": {
              "href": {
                "type": "string",
                "format": "url",
                "description": "URL to retrieve license",
                "example": "http://localhost:8080/users/{license-id}"
              }
            }
          }
        }
      },
      "licenseInquire_links": {
        "type": "object",
        "readOnly": true,
        "properties": {
          "self": {
            "type": "object",
            "readOnly": true,
            "properties": {
              "href": {
                "type": "string",
                "format": "url",
                "description": "URL to retrieve licenseInquire",
                "example": "http://localhost:8080/licenseInquires/{licenseInquire-id}"
              }
            }
          }
        }
      }
    },
    "parameters": {
      "license-inquire-id": {
        "name": "license-inquire-id",
        "in": "path",
        "required": true,
        "schema": {
          "type": "integer",
          "minimum": 1,
          "format": "int32",
          "description": "ID of license inquire resource"
        }
      },
      "page": {
        "name": "page",
        "in": "query",
        "schema": {
          "type": "integer",
          "minimum": 0,
          "format": "int32",
          "description": "Page number of pagination reqest"
        }
      },
      "size": {
        "name": "size",
        "in": "query",
        "schema": {
          "type": "integer",
          "minimum": 1,
          "format": "int32",
          "description": "Size of one page of pagination request"
        }
      }
    },
    "responses": {
      "license-created": {
        "description": "License Created",
        "content": {
          "application/json": {
            "schema": {
              "$ref": "#/components/schemas/LisenceResponseDTO"
            }
          }
        }
      }
    },
    "examples": {
      "customor_user_example": {
        "summary": "user example",
        "value": {
          "email": "sombody@gmail.com",
          "authority": "USER"
        }
      },
      "inquirer_user_example": {
        "summary": "inquirer example",
        "value": {
          "email": "somePartner@outlook.com",
          "authoirty": "PARTNER"
        }
      },
      "admin_user_example": {
        "summary": "admin example",
        "value": {
          "email": "raecheol.park@seculetter.com",
          "authority": "ADMIN"
        }
      }
    }
  }
}
```

