# Fair Candidate Selection  

Nidhish Nerur & Sandra You 

Optimization Methods Final Project (MIT 15.C57)

**Introduction**: Our project aims to address the challenge of hiring candidates with high performance potential while ensuring diversity. Specifically, we perform subset selection to identify 50 candidates for a hypothetical job opening. Companies struggle to select the most qualified candidates who satisfy job description requirements such as experience level and technical skills while balancing equitable representation. The objective of our subset selection is to maximize the performance potential of the selected group and ensure representation across gender, race, age, and other factors.

The baseline best performance model simply selects the top 50 candidates by their performance score and we show the relevant distributions of gender (0: Female, 1: Male) and race (0: Asian, 1: Black, 2: White, 3: Other, 4: Hispanic), where gender is colored pink for female and blue for male:


<img width="625" alt="Screenshot 2024-12-20 at 4 09 30 PM" src="https://github.com/user-attachments/assets/522e9f29-d929-49fd-a69a-daac5c9660a7" />

There is a clear gender divide, with 37 women and 13 men selected. In addition, white individuals represent the majority, followed by blacks, and ther appear to be discrepancies between the racial groups. The overall dataset has roughly even proportions of gender and racial groups, so we can directly inspect differences in raw counts. To further our results, we look at the intersectionality between race and gender:

<img width="550" alt="Screenshot 2024-12-20 at 4 11 52 PM" src="https://github.com/user-attachments/assets/ba2af920-7a26-4636-89ed-a09beee697a5" />

The benchmark model achieves the highest average performance score, yet fails to consider equality and fairness between demographic groups. Consequently, we wanted to use candidate and job data to create a rigorous optimization formulation that balances performance and fairness.

**Data Description**: We used both synthetic job listings and candidate information to perform our fair candidate selection. The Job Dataset contains specific hypothetical job opening(s) with relevant descriptors that the company is seeking in a particular candidate (Rana, 2023b). The features for the job dataset are discussed here: 

- **Min Exp**: Minimum years of experience required for the job
- **Max Exp**: Maximum years of experience allowed for the job
- **QualificationMapped**: The education level for the job in standardized categories
- **Min Salary**: The minimum salary offered for the job
- **Max Salary**: The maximum salary offered for the job
- **WorkMapped**: The type of work status, such as full-time, contract, part-time
- **PythonCode**: Whether Python proficiency is required for the job
- **JavaScriptCode**: Whether JavaScript proficiency is required for the job
- **SocialMediaSkill**: Whether social media management is expected for the role
- **CADSoftwareSkill**: Whether expertise in CAD (Computer-Aided Design) software is necessary for the job
- **NetworkDesignSkill**: Whether skills in network design are needed for the job

The Employee Dataset contains 3000 employee records and provides key features about the candidates’ current role and level of expertise (Rana, 2023a). Both datasets are synthetic but they are meant to mirror real-world candidate and job details. The final employee dataset used includes these features: 

- **YearsWorked**: Number of years the candidate has worked
- **TitleMapped**: Mapped job title, standardized for various roles
- **EducationMapped**: The standardized education level of the candidate
- **EmployeeTypeMapped**: Employment type, such as full-time, part-time, or contract
- **Age**: Age of the candidate to analyze trends related to demographics
- **GenderMapped**: The gender of the candidate, mapped into categories for diversity analysis
- **RaceMapped**: The standardized race of the candidate
- **PerformanceScoreMapped**: The candidate’s performance score, mapped to categories such as exceeds expectations, meets expectations, or needs improvement
- **PythonCode**: Inferred skill level in Python programming
- **JavaScriptCode**: Inferred skill level in JavaScript programming
- **SocialMediaSkill**: The candidate’s proficiency in social media management
- **NetworkDesign**: Inferred expertise in designing and managing computer networks
- **CADSoftwareSkill**: Proficiency in using CAD (Computer-Aided Design) software

The PerformanceScoreMapped column is mapped to random integers within specific ranges based on the category values: Exceeds (70-100), Fully Meets (50-70), Needs Improvement (20-50), and PIP (0-20). The PIP category indicates the employee is likely to be fired unless he or she significantly improves performance.

**Key Results**: We develop a fairness metric and optimization formulation to assess the trade-off between performance and fairness. These are discussed in greater detail in the attached project report. First, we adjust the weight associated with the gender constraint, which represents the minimum proportion of male candidates the model is enforced to select. Then, we plot the relationship between the performance score and fairness for the different gender weights, where each datapoint is for a particular gender weight:

<img width="492" alt="Screenshot 2024-12-20 at 4 14 25 PM" src="https://github.com/user-attachments/assets/b9759c76-1ba9-4523-b5ee-1142457b83cf" />

Contrary to our initial expectations, we see that generally as the performance score increases, the fairness metric tends to rise as we adjust the gender proportion. The graph indicates the gender proportion can have a net improvement in both fairness and performance. However, after a certain performance level, the fairness metric begins to decline, perhaps suggesting a trade-off at higher performance levels. Subsequently, we wanted to further investigate the interplay between the gender penalty and the other fairness considerations:

<img width="517" alt="Screenshot 2024-12-20 at 4 15 54 PM" src="https://github.com/user-attachments/assets/a339e8c2-f9ed-4d22-a21d-101c9af8f06d" />

The higher age, race, and intersection penalty scores indicate greater violations for the particular fairness consideration. As we increase the gender weight on the x-axis, the model is forced to select more male candidates, and we see the associated changes in the other penalties. In the graph we see that the age and intersectionality of race and gender stay somewhat stable, but the race penalty continues to increase as we require a greater proportion of males to be selected. We repeat this procedure by modifying the race weight independently and plotting the associated performance-fairness curve:

<img width="551" alt="Screenshot 2024-12-20 at 4 18 09 PM" src="https://github.com/user-attachments/assets/7a4727c1-f022-406a-ba51-6e25c76b5242" />

Here, we see a clear trade-off as performance increases, the fairness is somewhat lower with different racial proportions. The perfect scenario matches the population distribution of about 20% in each racial group. The graph indicates that accounting for racial discrepancies may result in greater penalties across the other fairness considerations, so we look at the respective changes across age, gender, and intersectionality scores:

<img width="505" alt="Screenshot 2024-12-20 at 4 19 42 PM" src="https://github.com/user-attachments/assets/6df5246e-cee9-44f1-b38e-1eca84ba6926" />

As the race weight increases to create more equal groups, the gender and age penalties seem to decline while the intersection penality rises. Even though the overall gender penalty declined, ensuring fairness at the intersection of race and gender is more challenging to uphold. As a result, we see trade-offs in optimizing fairness across multiple dimensions. 

**Impact**: We have developed a framework that maintains a strong balance between performance and fairness across different jobs, while including key domain-specific features. With our rigorous optimization formulation and fairness considerations, our model aims to achieve demographic parity across race, gender, intersectionality, and age. Accounting for these fairness measures is vital to fostering diversity and enhancing organizational inclusivity. We hope to minimize bias in the interview selection and hiring processes, providing equitable access to all.

Naturally, it is quite challenging to perfectly optimize for fairness across all dimensions while attaining the best performing candidates. This was evident in our findings as we adjusted the gender and race weights to see how our model adapts. Our work can be applied across industries to support data-driven, fair decision-making practices in human resources. Fair candidate selection is a difficult task as we have to prioritize multiple conflicting objectives, and our model formulation is just the first step to creating more equitable hiring practices.

