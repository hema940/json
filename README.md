# json
new repo
import React, { useState } from "react";
import Form from "@rjsf/core"; // react-jsonschema-form
import styled from "styled-components";

const Container = styled.div`
  display: flex;
  height: 100vh;
`;

const Editor = styled.div`
  flex: 1;
  padding: 20px;
  display: flex;
  flex-direction: column;
`;

const TextArea = styled.textarea`
  flex: 1;
  width: 100%;
  font-family: monospace;
  font-size: 14px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
`;

const FormContainer = styled.div`
  flex: 1;
  padding: 20px;
  border-left: 1px solid #ccc;
`;

const Error = styled.div`
  color: red;
  margin-top: 10px;
`;

const App = () => {
  const [jsonSchema, setJsonSchema] = useState({
    title: "Example Form",
    type: "object",
    properties: {
      name: { type: "string", title: "Name" },
      age: { type: "number", title: "Age" },
    },
  });

  const [schemaString, setSchemaString] = useState(JSON.stringify(jsonSchema, null, 2));
  const [error, setError] = useState("");

  const handleSchemaChange = (e) => {
    const newSchema = e.target.value;
    setSchemaString(newSchema);
    try {
      const parsedSchema = JSON.parse(newSchema);
      setJsonSchema(parsedSchema);
      setError("");
    } catch (err) {
      setError("Invalid JSON Schema");
    }
  };

  return (
    <Container>
      <Editor>
        <h3>JSON Schema Editor</h3>
        <TextArea
          value={schemaString}
          onChange={handleSchemaChange}
          rows={20}
        />
        {error && <Error>{error}</Error>}
      </Editor>
      <FormContainer>
        <h3>Generated Form</h3>
        <Form schema={jsonSchema} />
      </FormContainer>
    </Container>
  );
};

export default App;
