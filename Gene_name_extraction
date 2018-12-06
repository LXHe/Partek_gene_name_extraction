import pandas as pd
from sklearn.feature_extraction.text import CountVectorizer

gene_name = pd.read_excel('demo_gene_extraction.csv', index_col="Probeset ID")

# Drop Gene Regions with NaN value
gene_name_dp = gene_name.dropna(axis=0, subset=['Gene Region'])
# Fill NaN value in CpG Region with "OpenSea"
gene_name_dp['CpG Region'].fillna("OpenSea", inplace=True)
========================================================================================================================================
def my_tok(txt):
    return [x for x in txt.split(",")]

def gn_extract(DataSet, GeneRegion):
    gr_select = DataSet[DataSet['Gene Region'].str.match(GeneRegion, na=False)]
    gr_select_spt = gr_select['Gene Region'].str.split(";", expand=True)
    gn_select_spt = gr_select['Gene'].str.split(";", expand=True)

    mask = gr_select_spt == GeneRegion
    gn_select_msk = gn_select_spt[mask]
    # Drop duplicate gene names
    trans = gn_select_msk.T
    gn_dup = pd.DataFrame()
    for probe in gr_select.index.values:
        dup = trans[[probe]].drop_duplicates(subset=[probe])
        gn_dup = gn_dup.append(dup.T)
        
    gn_dup['Gene Name'] = gn_dup.apply(
        lambda x: ",".join(x.dropna()), axis=1
        )
    
    # Extract gene name   
    vectorizer = CountVectorizer(tokenizer=my_tok, lowercase=False)
    vectorizer.fit_transform(gn_dup['Gene Name'])
    gn = vectorizer.get_feature_names()

    df_final = pd.merge(
        DataSet[["CHR","CpG Region"]], gn_dup[['Gene Name']],
        left_index=True, right_index=True
        )
    return gn, df_final
========================================================================================================================================
gr_list = ["TSS1500", "TSS200", "5'UTR", "1stExon", "Body", "3'UTR"]
gn_dic = {}
df_dic = {}
for i in gr_list:
    gn_dic["{}_gn".format(i)], df_dic["{}_df".format(i)] = gn_extract(DataSet=gene_name_dp, GeneRegion=i)
========================================================================================================================================
# Output as an excel file with gene regions on seperated excel sheets 
writer = pd.ExcelWriter('SummaryTable.xlsx', engine='xlsxwriter')

for key, value in df_dic.items():
    value.to_excel(writer,sheet_name='{}'.format(key))
writer.save()
