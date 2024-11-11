# auth

```code
npx create-next-app@14
```

https://www.creative-tim.com/twcomponents/cheatsheet/

input field
```code
'use client'
import { FieldHookConfig, useField } from "formik";


interface InputFieldProps extends React.InputHTMLAttributes<HTMLInputElement> {
    label: string;
    isUpdate?: boolean;
}

export const InputField: React.FC<InputFieldProps> = ({ label, isUpdate = false, ...props }) => {
    const [field, meta] = useField(props as FieldHookConfig<string>);

    return (
        <fieldset className={`p-4 border text-black rounded ${isUpdate ? 'bg-gray-100' : ''}`}>
            <label htmlFor={props.id || props.name} className="block text-sm font-medium text-gray-700 mb-2">
                {label}
            </label>
            <input
                className={`w-full p-2 border ${meta.touched && meta.error ? 'border-red-500' : 'border-gray-300'} rounded`}
                {...field}
                {...props}
            />
            {meta.touched && meta.error ? (
                <div className="text-red-500 text-sm mt-1">{meta.error}</div>
            ) : null}
        </fieldset>
    );
}

export default InputField;
```

form
```code
'use client'
import * as Yup from 'yup';
import { Formik, Form } from "formik";
import Link from "next/link";
import { InputField } from "./input-field";

export const LoginForm = () => {

  const validationSchema = Yup.object({
    identifier: Yup.string()
      .required('This field is required')
      .test('identifier', 'Invalid email or username', (value) => {
        if (value) {
          const usernameRegex = /^[\w_]{3,30}$/;
          if (usernameRegex.test(value)) {
            return true;
          }
          const emailRegex = /^[\w._-]+@[\w.]+\.[\w]{2,4}$/;
          if (emailRegex.test(value)) {
            return true;
          }
        }
        return false;
      }),
    password: Yup.string()
      .required('This field is required')
      .min(5, 'Password must be at least 5 characters long')
  });

  const submit = async (data: any) => {
    console.log(data);
    /*
    const response = await fetch(
    "http://localhost:8006", {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify(data),
})
    */

  };

  return (
    <div>
      <div>
        <Formik initialValues={{
          identifier: '',
          password: '',
        }}
          validationSchema={validationSchema}
          onSubmit={async (values, { setSubmitting }) => {
            await submit(values);
            setSubmitting(false);
          }}
        >
          {({ isSubmitting }) => (
            <Form>
              <h1>LOG IN</h1>
              <InputField label='Email or username' name='identifier' type='text' />
              <InputField label='Password' name='password' type='password' />
              <div>
                Don't have an account ?
                <Link href='/register'>Register</Link>
              </div>
              <button type='submit' disabled={isSubmitting}>Log In</button>
            </Form>
          )}
        </Formik>
      </div>
    </div>
  )
}

export default function Home() {
  return (
    <div>
      <div>
        <LoginForm />
      </div>
    </div>
  );
}
```


prop
```code
server.port=8006
spring.application.name=test


spring.jpa.hibernate.ddl-auto=update

spring.datasource.url=jdbc:mysql://localhost:3306/testdb
spring.datasource.username=root
spring.datasource.password=123456
```




