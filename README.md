[![Dart](https://github.com/Missing-Tech/transloadit-flutter-sdk/actions/workflows/dart.yml/badge.svg?branch=main)](https://github.com/Missing-Tech/transloadit-flutter-sdk/actions/workflows/dart.yml)

# Transloadit Flutter SDK

Flutter integration with [Transloadit](https://transloadit.com/)

# WIP - 1.0 Release

- [x] Create assemblies
- [x] Get assemblies
- [x] Delete assemblies
- [x] Add files
- [x] Remove files
- [x] Replay assemblies
- [x] Retrieve list of assemblies
- [x] Replay assembly notification
- [x] Retrieve list of assembly notifications
- [x] Retrieve month's bill
- [x] Get priority job slots
- [x] Create templates
- [x] Get templates
- [x] Edit templates
- [x] Delete templates
- [x] Run templates
- [x] Error handling
- [x] Better tests
- [x] Add an example file

## Basic Examples

### Creating client
```dart
TransloaditClient client = TransloaditClient(
        authKey: 'KEY',
        authSecret: 'SECRET');
```

### Getting an assembly
```dart
TransloaditResponse response = await client.getAssembly(
        assemblyID: 'ASSEMBLY_ID');
print(response.statusCode) // 200
```

### Creating an assembly
```dart
TransloaditAssembly assembly = client.newAssembly();
final imagePath = 'assets/cat.jpg';

assembly.addStep("import", "/http/import",
        {"url": "https://demos.transloadit.com/inputs/chameleon.jpg"});
assembly.addStep("resize", "/image/resize", {"height": 400});

TransloaditResponse response = await assembly.createAssembly();

print(response['ok']) // "ASSEMBLY_COMPLETED"
```

### Creating a template
```dart
TransloaditTemplate template = client.newTemplate(name: "template");

template.addStep("import", "/http/import",
          {"url": "https://demos.transloadit.com/inputs/chameleon.jpg"});
template.addStep("resize", "/image/resize", {"use": "import", "height": 400});

TransloaditResponse response = await template.createTemplate();

print(response['ok']) // "TEMPLATE_CREATED"
```

### Updating a template
```dart
TransloaditResponse response = await client.updateTemplate(
        templateID: 'TEMPLATE_ID',
        template: {
          "import": {
            "robot": "/http/import",
            "url": "https://demos.transloadit.com/inputs/chameleon.jpg"
          },
          "resize": {"use": "import", "robot": "/image/resize", "height": 200}
        },
      );

print(response['ok']) // "TEMPLATE_UPDATED"
```

### Running template with fields
```dart
TransloaditAssembly assembly = client.runTemplate(
        templateID: 'TEMPLATE_ID', 
        params: {'fields': {'input': 'items.jpg'}});
TransloaditResponse response = await assembly.createAssembly();

print(response.data["ok"]); // "ASSEMBLY_COMPLETED"
```

### Running template with fields
```dart
TransloaditAssembly assembly = client.runTemplate(
        templateID: 'TEMPLATE_ID', 
        params: {'fields': {'input': 'items.jpg'}});
TransloaditResponse response = await assembly.createAssembly();

print(response.data["ok"]); // "ASSEMBLY_COMPLETED"
```

### Tracking upload progress
```dart
TransloaditResponse response = await assembly.createAssembly(
        onProgress: (progressValue) {
          print(progressValue); // Value from 0-100
        },
        onComplete: () {
          // Do stuff
        }),
      );
```