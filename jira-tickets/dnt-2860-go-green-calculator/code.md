# 🖥 Code

```javascript
// Data.js & stories.js


/* eslint-disable */
const data = {
  uid: '63dd00eb-8fd8-48da-a701-c30ef2579afe',
  componentName: 'Simple Calculator',
  dataSource: '{CF80964E-AE81-41D3-92C4-B4E9E5C64283}',
  fields: {
    Tabs: [
      {
        id: 'ee0e6555-b17a-4377-89e3-1db15564ce81',
        fields: {
          'Input Description': {
            value:
              '<p>Enter your average monthly usage in kWh to calculate how many blocks are required to match. (Tip: Check your bill or sign in <a href="/my-account/sign-in">here</a>&nbsp;to find your average monthly usage.)</p>',
          },
          'Input Fields': [
            {
              id: 'adb41397-987e-4fa9-9069-f971569a134a',
              fields: {
                'Input Min': {
                  value: '200',
                },
                'Input Max': {
                  value: '',
                },
                'Input Explanation': {
                  value: 'One GoGreen program block represents 100 kWh of clean energy.',
                },
                'Input Min Warning': {
                  value: 'The minimum kWh that can be entered is ',
                },
                'Input Label': {
                  value: 'kWh per month',
                },
                'Input ID': {
                  value: '1',
                },
                'Input Type': {
                  value: 'Input',
                },
                'Input Description': {
                  value: 'Enter average monthly kWh usage here',
                },
              },
            },
          ],
          'Output Calculations': [
            {
              id: '3817967b-05bb-443f-b389-8466be741b5c',
              fields: {
                'Output Font Size': {
                  value: 'Small',
                },
                'Output Calculation': {
                  value: '{1}/100',
                },
                'Output Calculation Format': {
                  value: 'Currency',
                },
                'Output Presentation': {
                  value: '${output} | Monthly Rate',
                },
              },
            },
            {
              id: 'f7f5d0e6-885c-4232-9499-1a66acf6044b',
              fields: {
                'Output Font Size': {
                  value: 'Big',
                },
                'Output Calculation': {
                  value: '{1}/100',
                },
                'Output Calculation Format': {
                  value: 'Integer',
                },
                'Output Presentation': {
                  value: '{output} Blocks',
                },
              },
            },
          ],
          'Background Image': {
            value: {
              src: 'https://scdev05.duke-energy.com:443/_/media/images/carousel/carousel-surge-protection.jpg?h=320&la=en&w=320',
              alt: 'Image',
              width: '320',
              height: '320',
            },
          },
          'Output Description': {
            value: '',
          },
          'Tab Title': {
            value: 'Match Electric Usage',
          },
        },
      },
      {
        id: '69bdcdcb-48d6-4a1d-ab70-d07c3eec3993',
        fields: {
          'Input Description': {
            value:
              "<p>$ Calculate Based on Monthly Payment</p>\n<p>Enter what you'd like to pay each month to calculate how many blocks to purchase.</p>",
          },
          'Input Fields': [
            {
              id: 'ba7c0d1e-18c0-4f11-9b93-8f9c09164306',
              fields: {
                'Input Min': {
                  value: '',
                },
                'Input Max': {
                  value: '',
                },
                'Input Explanation': {
                  value: 'One GoGreen Ohio program block represents 100 kWh of clean energy.',
                },
                'Input Min Warning': {
                  value: 'The minimum amount that can be entered is $',
                },
                'Input Label': {
                  value: 'Dollars per Month',
                },
                'Input ID': {
                  value: '2',
                },
                'Input Type': {
                  value: 'Input',
                },
                'Input Description': {
                  value: 'Enter monthly budgeted program payment here',
                },
              },
            },
          ],
          'Output Calculations': [
            {
              id: '8ae8fedb-1261-4c07-8ed6-4d10fc122e34',
              fields: {
                'Output Font Size': {
                  value: 'Big',
                },
                'Output Calculation': {
                  value: '{2}/3',
                },
                'Output Calculation Format': {
                  value: 'Integer',
                },
                'Output Presentation': {
                  value: '{output} Blocks',
                },
              },
            },
          ],
          'Background Image': {
            value: {
              src: 'https://scdev05.duke-energy.com:443/_/media/images/carousel/carousel-surge-protection.jpg?h=320&la=en&w=320',
              alt: 'Image',
              width: '320',
              height: '320',
            },
          },
          'Output Description': {
            value: '',
          },
          'Tab Title': {
            value: 'Choose Your Payment',
          },
        },
      },
    ],
    Subhead: {
      value:
        "Use this calculator to estimate how many blocks are required to match your electricity usage as well as to calculate the amount of blocks you'll receive based on your monthly payment.",
    },
    Icon: {
      value: {},
    },
    HeadLine: {
      value: 'Go Green Calculator',
    },
  },
};

export default data;

import React from 'react';
import SimpleCalculator from './index';
import { compositionFunction } from './composition';
import Data from './data';

const props = compositionFunction(Data);

export default {
  title: 'Components/SimpleCalculator',
  component: SimpleCalculator,
};

const Template = args => {
  return <SimpleCalculator {...args} />;
};

export const Primary = Template.bind({});

Primary.args = props;




```



{% tabs %}
{% tab title="index.tsx" %}
```tsx
//-------------------start------------------------------
import { useEffect, useState } from 'react';
import Input from 'src/components/Calculator/inputComponents/Input';
import CallToAction from 'src/components/CallToAction';
import RichText from 'src/components/RichText';
import SegmentedControl from 'src/components/SegmentedControl';
import MoneyLabelRow from '../Calculator/outputComponents/MoneyLabelRow';
import LazyImage from '../LazyImage/';
import { SimpleCalculatorTabsType, SimpleCalculatorType } from './types';

const createEquation = (operator: string, firstNum: string, secondNum: string) => {
  if (operator === '/') {
    return parseInt(firstNum) / parseInt(secondNum);
  }
  if (operator === '*') {
    return parseInt(firstNum) * parseInt(secondNum);
  }
  if (operator === '+') {
    return parseInt(firstNum) + parseInt(secondNum);
  }
  if (operator === '-') {
    return parseInt(firstNum) - parseInt(secondNum);
  }
  return 0;
};

const CalculatorPanel = ({
  index,
  setInputStates,
  inputStates,
  ...tab
}: SimpleCalculatorTabsType) => {
  const onChange = (event: string, index: number) => {
    const inputStatesCopy = [...inputStates];
    inputStatesCopy[index] = event;
    setInputStates(inputStatesCopy);
  };

  const formatMoney = (money: string) =>
    `$ ${money.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')}`;
  const inputVal = index === 1 ? formatMoney(inputStates[index]) : inputStates[index];

  return (
    <div className=" pt-24">
      <RichText field={tab.panelDescription} />
      <h3 className="font-bold pt-24">{tab.inputDescription}</h3>
      <Input
        label={index === 1 ? 'Monthly payment' : 'kWh/per month'}
        prompt=""
        value={inputVal}
        valueChanger={event => {
          onChange(event, index);
        }}
      />
      <h4
        className={`${
          Number(inputStates[index]) < tab.inputMin ? 'text-red' : ''
        } py-32 text-lg font-bold`}
      >
        {`${tab.inputMinWarning}${tab.inputMin} ${tab.inputLabel}`}
      </h4>
      <h4 className={'py-16 text-lg font-bold'}>{`${tab.inputExplanation}`}</h4>
    </div>
  );
};

const evaluateEquation = (equationString: string, inputState: string) => {
  const variableNumeratorRemoved = equationString.substring(3);
  const operator = variableNumeratorRemoved.charAt(0);
  const demominator = variableNumeratorRemoved.substring(1);
  const val = Math.round(createEquation(operator, inputState, demominator));
  return val;
};

const SimpleCalculatorComponent = ({ tabs }: SimpleCalculatorType) => {
  const [inputStates, setInputStates] = useState(['6000', '6000']);
  const [activeTab, setActiveTab] = useState(0);
  // block represents 100 kWh
  const [firstPanelCalculation, setFirstPanelCalculation] = useState(0);
  const [secondPanelCalculation, setSecondPanelCalculation] = useState(0);
  const segmentedControlOptions = tabs.map(tab => tab.tabTitle);

  // Sync panel 1 output
  useEffect(() => {
    const outputValue = evaluateEquation(tabs[0].outputCalculation, inputStates[0]);
    setFirstPanelCalculation(outputValue);
  }, [inputStates[0]]);

  // Sync panel 2 output
  useEffect(() => {
    const outputValue = evaluateEquation(tabs[1].outputCalculation, inputStates[1]);
    setSecondPanelCalculation(outputValue);
  }, [inputStates[1]]);

  return (
    <div className="w-full container-4xl overflow-hidden border rounded-md my-24 border-gray-light sm:flex">
      <div className=" sm:w-3/5 p-24">
        <SegmentedControl
          segmentedControlOptions={segmentedControlOptions}
          tabSelect={(val: number) => setActiveTab(val)}
        >
          {tabs.map((tab, index) => (
            <CalculatorPanel
              key={index}
              {...tab}
              index={index}
              setInputStates={setInputStates}
              inputStates={inputStates}
            />
          ))}
        </SegmentedControl>
      </div>
      <div className="sm:w-2/5 ">
        <div className="relative bg-blue py-64 h-full sm:flex sm:flex-col sm:justify-center text-center items-center text-white">
          <LazyImage
            src={tabs[0].backgroundImage.toString()}
            alt=""
            className="absolute left-0 top-0  h-full w-full  object-cover  object-bottom"
          />
          <div className="relative">
            {activeTab === 0 && (
              <>
                <MoneyLabelRow
                  number={60}
                  label={'Monthly Rate'}
                  textColorClass="white"
                  borderColorClass="border-gray"
                />
                <h2 className="mx-auto text-3xl-fixed text-center">
                  {firstPanelCalculation || 0} Blocks
                </h2>
              </>
            )}
            {activeTab === 1 && (
              <h2 className="mx-auto text-2xl-fixed text-center">
                {secondPanelCalculation || 0} Blocks
              </h2>
            )}
          </div>
        </div>
      </div>
    </div>
  );
};

const SimpleCalculator = ({ tabs, ...rest }: SimpleCalculatorType) => {
  return (
    <CallToAction {...rest}>
      <SimpleCalculatorComponent tabs={tabs} />
    </CallToAction>
  );
};

export default SimpleCalculator;


//-------------------end-------------------------------
```
{% endtab %}

{% tab title="composition.tsx" %}
```
//-------------------start------------------------------
/* eslint-disable @typescript-eslint/no-unused-vars */
import Composition from 'src/lib/Composition';
import Data from 'src/components/SimpleCalculator/data';
import { compositionFunction as CallToActionComposition } from 'src/components/CallToAction/composition';
import SimpleCalculator from '.';
import { SimpleCalculatorType } from './types';

const { compositionFunction, component } = Composition(SimpleCalculator)(({ fields }) => {
  const CTA = CallToActionComposition({ fields });

  const reduceItems = (tabs: typeof fields): SimpleCalculatorType['tabs'] => {
    return fields?.Tabs?.reduce((acc: SimpleCalculatorType['tabs'], curr: any) => {
      return [
        ...acc,
        {
          panelDescription: curr.fields?.['Input Description'].value,
          inputLabel: curr.fields?.['Input Fields'][0].fields['Input Label'].value,
          inputId: curr.fields?.['Input Fields'][0].fields['Input ID'].value,
          inputType: curr.fields?.['Input Fields'][0].fields['Input Type'].value,
          inputDescription: curr.fields?.['Input Fields'][0].fields['Input Description'].value,
          outPutFontSize: curr.fields?.['Output Calculations'][0].fields['Output Font Size'].value,
          outputCalculation:
            curr.fields?.['Output Calculations'][0].fields['Output Calculation'].value,
          outputCalculationFormat:
            curr.fields?.['Output Calculations'][0].fields['Output Calculation Format'].value,
          outputPresentation:
            curr.fields?.['Output Calculations'][0].fields['Output Presentation'].value,
          backgroundImage: curr.fields?.['Background Image'].value.src,
          tabTitle: curr.fields?.['Tab Title'].value,
          inputExplanation: curr.fields?.['Input Fields'][0].fields['Input Explanation'].value,
          inputMinWarning: curr.fields?.['Input Fields'][0].fields['Input Min Warning'].value,
          inputMin: curr.fields?.['Input Fields'][0].fields['Input Min'].value,
        },
      ];
    }, []);
  };

  return {
    ...CTA,
    tabs: reduceItems(fields?.Tabs),
  };
});

export { compositionFunction, component as default };


//-------------------end-------------------------------
```
{% endtab %}

{% tab title="test.tsx" %}
```javascript
//-------------------start------------------------------

/* eslint-disable max-lines */
import '@testing-library/jest-dom';
// import { ComponentEvent } from 'src/lib/Analytics/gtm/types';
import { fireEvent, renderWithCTX, screen } from 'src/lib/testWrappers';
import { compositionFunction } from './composition';
import Data from './data';
import SimpleCalculator from './index';
jest.mock('src/lib/useIntersection');
jest.mock('src/lib/useMediaQuery');
const props = compositionFunction(Data);
describe('SimpleCalculator', () => {
  it('should render', () => {
    renderWithCTX(<SimpleCalculator {...props} />);
    screen.logTestingPlaygroundURL();
    const titleHeading = screen.getByRole('heading', { name: /go green calculator/i });
    const inputDescription = screen.getByText(
      /use this calculator to estimate how many blocks are required to match your electricity usage as well as to calculate the amount of blocks you'll receive based on your monthly payment\./i
    );
    const tabTitle = screen.getByRole('tab', { name: /match electric usage/i });
    const choosePayment = screen.getByRole('tab', { name: /choose your payment/i });
    const exp = screen.getByText(
      /enter your average monthly usage in kwh to calculate how many blocks are required to match\. \(tip: check your bill or sign in to find your average monthly usage\.\)/i
    );
    const input = screen.getByRole('textbox', { name: /kwh\/per month/i });
    const inputLabel = screen.getByText(/kwh\/per month/i);
    const inputMinWarning = screen.getByRole('heading', {
      name: /the minimum kwh that can be entered is 200 kwh per month/i,
    });
    // block explinations should differ but as of right now they are the same in sitecore and the data file.
    const inputExplinations = screen.getAllByText(
      /one gogreen ohio program block represents 100 kwh of clean energy\./i
    );
    const output = screen.getByRole('heading', {
      name: /60 blocks/i,
    });
    expect(titleHeading).toBeInTheDocument();
    expect(inputDescription).toBeInTheDocument();
    expect(tabTitle).toBeInTheDocument();
    expect(output).toBeInTheDocument();
    expect(choosePayment).toBeInTheDocument();
    expect(input).toBeInTheDocument();
    expect(inputMinWarning).toBeInTheDocument();
    expect(inputExplinations[0]).toBeInTheDocument();
    expect(inputLabel).toBeInTheDocument();
    expect(exp).toBeInTheDocument();
  });

  it('sets the active tab on click', () => {
    renderWithCTX(<SimpleCalculator {...props} />);
    const choosePaymentTab = screen.getByRole('tab', {
      name: /choose your payment/i,
    });
    const matchElectricInputDescription = screen.getByRole('heading', {
      name: /enter average monthly kwh usage here/i,
    });
    const choosePaymentInputDescription = screen.getByRole('heading', {
      name: /Enter monthly budgeted program payment here/i,
    });
    const tabs = screen.queryAllByRole('tab');
    const matchElectricPrompt = tabs[0];
    const choosePaymentPrompt = tabs[1];
    expect(choosePaymentTab).toBeInTheDocument();
    expect(matchElectricInputDescription).toBeInTheDocument();
    expect(choosePaymentInputDescription).toBeInTheDocument();
    expect(choosePaymentInputDescription.closest('#panel-choose-your-payment')).toHaveClass(
      'hidden'
    );
    expect(choosePaymentPrompt).toBeInTheDocument();
    expect(matchElectricPrompt).toBeInTheDocument();
    expect(choosePaymentPrompt).toHaveAttribute('aria-selected', 'false');
    fireEvent.click(choosePaymentPrompt);
    expect(choosePaymentPrompt).toHaveAttribute('aria-selected', 'true');
  });

  it('should should change the color class of input min warning when the input is insufficiently large', () => {
    renderWithCTX(<SimpleCalculator {...props} />);
    const input = screen.getByRole('textbox', {
      name: /kwh\/per month/i,
    });
    const inputMinWarning = screen.getByRole('heading', {
      name: /the minimum kwh that can be entered is 200 kwh per month/i,
    });
    fireEvent.change(input, { target: { value: 6000 } });
    expect(input).toHaveAttribute('value', '6000');
    expect(inputMinWarning).not.toHaveClass('text-red');
    fireEvent.change(input, { target: { value: 2 } });
    expect(inputMinWarning).toHaveClass('text-red');
  });
});

//-------------------end-------------------------------
```
{% endtab %}

{% tab title="types.tsx" %}
```tsx
//-------------------start------------------------------
import { CallToActionProps } from 'src/components/CallToAction/types';
import { ComponentEvent } from 'src/lib/Analytics/gtm/types';

interface SimpleCalculatorTabsType {
  inputFields: any;
  analytics: ComponentEvent;
  panelDescription: string;
  index: number;
  inputLabel: string;
  inputId: string;
  inputType: string;
  inputMin: number;
  inputMinWarning: string;
  inputExplanation: string;
  outPutFontSize: string;
  inputDescription: string;
  outputCalculation: string;
  outputCalculationFormat: string;
  outputPresentation: string;
  backgroundImage: JSS.ImageField;
  tabTitle: string;
  setInputStates: React.Dispatch<React.SetStateAction<string[]>>;
  inputStates: Array<string>;
}

interface SimpleCalculatorType {
  tabs: Array<SimpleCalculatorTabsType>;
}

type SimpleCalculatorComponentType = SimpleCalculatorType & CallToActionProps;

export type { SimpleCalculatorComponentType, SimpleCalculatorType, SimpleCalculatorTabsType };


//-------------------end-------------------------------
```
{% endtab %}
{% endtabs %}
